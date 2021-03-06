# Tags

Tags allow you to add custom data points to profiles. They are used to label, sort, and filter profiles on data that is not typically collected through the application. Since tags are flexible, organizations can use them for many purposes, such as Cabin Numbers, Homeroom Assignments, Field Trip Activities, or almost any other custom metadata.

Tags are initially set up at the organization level, where they're referred to as "Group Tags". Applying an instance of a tag to a specific profile creates a "Profile Tag."

There are two types of tags: Free-Text and Multiple Choice.

Free-Text Group Tags are created with a `value` only, allowing for arbitrary text in the Profile Tag. For example, you could set up a Group Tag with the `value` 'Study Buddy', with the Profile Tag's `value` as the name of that profile's study buddy.

Conversely, a Multiple Choice tag might be more appropriate if your data has a pre-determined set of options. In Multiple Choice tags, several Group Tag entries are created: A "Parent" Group Tag, containing the `value`, and several "Option" Group Tags, each with a `parentID` property that relates them to the Parent Group Tag. The Profile Tag would then reference both the Parent Group Tag and the Option Group Tag selected for that profile. For example, if you were storing lodging data, you might create a 'Cabin Selection' Group Tag as the parent, with Option Tags of 'Cabin A' and 'Cabin B'. The Profile Tag would then reference the 'Cabin Selection' tag as the `tagID`, and the 'Cabin B' tag as the `optionID`.

## Group Tags
A Group Tag object contains the following fields:
 - `id` CampDoc's primary key for the tag
 - `groupID` Will auto-populate with your organization's ID
 - `parentID` For an Option Tag, indicates which tag is the parent
 - `value` **[REQUIRED]** The label or value of the tag
 - `created` **[READ-ONLY]** Timestamp of when the tag was created
 - `updated` **[READ-ONLY]** Timestamp of when the tag was last updated
 - `deactivated` Timestamp of when the tag was deactivated. Deactivated tags will not appear in filters or reports

## Routes
```
GET    /v2/organizations/{orgID}/tags
POST   /v2/organizations/{orgID}/tags
GET    /v2/organizations/{orgID}/tags/{tagID}
PUT    /v2/organizations/{orgID}/tags/{tagID}
DELETE /v2/organizations/{orgID}/tags/{tagID}
```
### Read More
For more information on Group Tags, please [click here](/v2/chapters/tags-groups.md).


## Profile Tags
A Profile Tag object contains the following fields:
 - `id` CampDoc's primary key for the tag
 - `profileID` ID of the profile being assigned the tag
 - `tagID` **[REQUIRED]** ID of the top-level group tag this references
 - `optionID` If linking to a child group tag, ID of chosen child
 - `value` Value to display if it's a free-text tag
 - `created` **[READ-ONLY]** Timestamp of when the tag was created
 - `updated` **[READ-ONLY]** Timestamp of when the tag was last updated
 - `deactivated` Timestamp of when the tag was deactivated. Deactivated tags will not appear in reports.

## Routes
Note that the `{tagID}` used in these routes is the ID of the profile tag instance, not the group tag. This is the `id` property on each object returned by the `GET /{profileID}/tags` route.
```
GET    /v2/organizations/{orgID}/profiles/{profileID}/tags
POST   /v2/organizations/{orgID}/profiles/{profileID}/tags
GET    /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
PUT    /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
DELETE /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
```
### Read More
For more information on Profile Tags, please [click here](/v2/chapters/tags-profiles.md).


## Configuration
Assume we've created a group tag that looks like the following:
```json
{
  "id": 54321,
  "groupID": 12345,
  "parentID": null,
  "value": "Favorite Color"
}
```
As noted above, tags can be multiple choice or free text. If we want free text profile tags associated with this one, there's no more configuration needed for the group tags; we can create a profile tag with the following POST request body:
```json
{
  "tagID": 54321,
  "value": "Blue"
}
```
This will let us use the List Builder to search for `Favorite Color => matches => 'blu'` and have profile 112233 returned.
If we instead want to have a multiple choice group tag setup, we create a new group tag for each desired option.
```json
{
  "id": 54322,
  "parentID": 54321,
  "value": "Red"
},
{
  "id": 54323,
  "parentID": 54321,
  "value": "Green"
},
{
  "id": 54324,
  "parentID": 54321,
  "value": "Blue"
}
```
Then, create a profile tag referencing the parent-child combination that we need.
```json
{
  "tagID": 54321,
  "optionID": 54324
}
```
This allows us to use the List Builder to search for `Favorite Color => is => 'Blue'` to, again, return profile 112233.

There are several considerations needed when creating group or profile tags. The routes will return error messages designed to help resolve creation issues. The validations that return these errors are listed here for reference.
- Group Tags
  - Must have a `value`
  - There can only be one level of children for each group tag. Cannot assign a child tag's `ID` as another tag's `parentID`.
  - Cannot create tags with duplicate `value`s and `parentID`s (e.g. If there's a top-level tag with `value` 'Favorite Color', you cannot create another top-level tag with that `value`. You could create a tag with that `value` but a different `parentID`)
  - `parentID` cannot be updated
- Profile Tags
  - Must have a `tagID` and either an `optionID` or a `value`. Cannot have both.
  - `tagID` must point to a top-level group tag.
  - A profile can only have one profile tag for each group tag.
  - Cannot assign an `optionID` that points to a tag that is not a child of the group tag referenced by `tagID`
  - Cannot create a free text profile tag with a `tagID` referencing a group tag that has children.

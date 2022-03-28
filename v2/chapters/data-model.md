# Data Model

The API supports create, read, update, and/or delete for [profiles](./profiles.md), [registrations](./registrations.md), [group tags](./tags-groups.md), and [profile tags](./tags-profiles.md). It supports create, read, and delete for [users](./users.md). It only supports read for [groups](./groups.md), [questions](./questions-answers.md), and [answers](./questions-answers.md).

The diagram below shows the inter-relationships between these entities.

```mermaid
erDiagram

profile {
  INTEGER profileId
  TEXT givenName
  TEXT middleName
  TEXT familyName
  TEXT dob
  TEXT sex
  TEXT phase
  TEXT identifier
  INTEGER completeness
}

user {
  INTEGER profileId
  TEXT email
  TEXT phone
  TEXT location
  TEXT givenName
  TEXT familyName
}

group {
  INTEGER groupId
  TEXT groupIdentifier
  TEXT name
  TEXT description
  INTEGER tuition
  MAP capacity
  INTEGER parentId
  ARRAY parents
  DATE start
  DATE finish
  DATE registrationOpen
  DATE registrationClose
  TEXT phase
  TIMESTAMP created
  TIMESTAMP updated
}

registration {
  INTEGER registrationId
  INTEGER profileId
  INTEGER groupID
  TEXT patient
  BOOLEALN waitlisted
  DATE created
  DATE updated
  TIMESTAMP deactivated
}

question {
  INTEGER questionId
  TEXT label
}

answer {
  INTEGER questionId
  INTEGER profileId
  TEXT value
  TIMESTAMP created
  TIMESTAMP updated
}

groupTag {
  INTEGER groupTagId
  INTEGER groupId
  INTEGER parentId
  TEXT value
  TIMESTAMP created
  TIMESTAMP updated
  TIMESTAMP deactivated
}

profileTag {
  INTEGER profileTagId
  INTEGER profileId
  INTEGER groupTagId
  TEXT value
  TIMESTAMP created
  TIMESTAMP updated
  TIMESTAMP deactivated
}

profile ||--o{ user : profileId
group ||--o{ group : parentId
group ||--o{ registration : groupId
profile ||--o{ registration : profileId
profile ||--o{ answer : profileId
question ||--o{ answer : questionId
group ||--o{ groupTag : groupId
groupTag ||--o{ groupTag : parentId
profile ||--o{ profileTag : profileId
groupTag ||--o{ profileTag : groupTagId

```
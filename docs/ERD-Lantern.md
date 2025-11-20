---
share_link: https://share.note.sx/f6p2zzqx#29ZQ0F9rCGsgvLozSqkN4Ro2Htm5+wyEMank6Q/7Sh0
share_updated: 2025-11-16T12:29:46+02:00
aliases:
---
## Database Schema: 

### 1. Entities

#### 1.1 publishers
| Column     | Type        | قيود / ملاحظات  |
| ---------- | ----------- | --------------- |
| auth_id    | UUID        | UNIQUE NOT NULL |
| name       | TEXT        | NOT NULL        |
|            |             |                 |
| avatar_url | TEXT        | اختياري         |
| bio        | TEXT        | اختياري         |
| created_at | TIMESTAMPTZ | DEFAULT now()   |
| updated_at | TIMESTAMPTZ | DEFAULT now()   |

#### 1.2 claims
| Column       | Type        | Notes                                     |
| ------------ | ----------- | ----------------------------------------- |
| id           | UUID        | PK                                        |
| publisher_id | UUID        | FK → publishers.auth_id ON DELETE CASCADE |
| title        | TEXT        | NOT NULL                                  |
| body         | TEXT        | markdown                                  |
| tags         | TEXT[]      | DEFAULT '{}' مصفوفة نصوص                  |
| image_url    | TEXT        | اختياري                                   |
| is_published | BOOLEAN     | DEFAULT true                              |
| created_at   | TIMESTAMPTZ | DEFAULT now()                             |
| updated_at   | TIMESTAMPTZ | DEFAULT now()                             |

---
## Endpoints

> انت دلوقت عندك، اثنين او ثلاثة endpoints خلينا نقول:
1. Claims: 
    - `https://www.domain.com/claims`
    - Claim( name or id ): `claims/{id}`
    - no need to login
    - simple list view
    - title , tags , img, posted-at

2. Publish:
    - `www.domain.com/publish`
    - `www.domain.com/publish/login`
    - `www.domain.com/publish/register`
    - need auth

4. API:
    - `www.domain.com/api`

---

## API - REST Schema

`/api/claims`
- GET: will return all claims
- POST: will add claim into claims

`/api/claims/claim?id={id}`
- GET: will get one claim with an id
- PUT: will update or edit claim
- DELETE: will delete the claim

`/api/publishers/`
- POST: will Create a new publisher

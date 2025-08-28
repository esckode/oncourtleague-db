# Tournament DB Design

The diagram below describes the current schema: Players, Teams, Season, Matches, and MatchParticipants. Fields marked PK are primary keys, FK are foreign keys.

```mermaid
erDiagram
    PLAYERS {
        INTEGER player_id PK
        VARCHAR first_name
        VARCHAR last_name
        DATE date_of_birth
        VARCHAR email
        VARCHAR phone_number
        INTEGER team_id FK
    }

    TEAMS {
        INTEGER team_id PK
        VARCHAR team_name
        VARCHAR coach_name
    }

    SEASON {
        INTEGER season_id PK
        INTEGER year
        VARCHAR season_name
        DATE begin_date
        DATE end_date
    }

    MATCHES {
        INTEGER match_id PK
        VARCHAR match_type
        INTEGER season_id FK
        TIMESTAMP match_date
        VARCHAR location
        INTEGER[] scores_winner
        INTEGER[] scores_runnerup
        VARCHAR winner_participant_type
        INTEGER winner_participant_id
    }

    MATCH_PARTICIPANTS {
        INTEGER match_id FK
        VARCHAR participant_type
        INTEGER participant_id
        INTEGER team_id FK
        SMALLINT side
        -- composite PK: (match_id, participant_type, participant_id)
    }

    TEAMS ||--o{ PLAYERS : "has"
    SEASON ||--o{ MATCHES : "includes"
    MATCHES ||--o{ MATCH_PARTICIPANTS : "has"
    TEAMS ||--
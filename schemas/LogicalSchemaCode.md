```mermaid
erDiagram
    PLAYER {
        BIGINT player_id PK
        VARCHAR nickname
        VARCHAR first_name
        VARCHAR last_name
        VARCHAR country
        BIGINT current_team_id FK
    }
    PLAYER_PROFILE {
        BIGINT profile_id PK
        BIGINT player_id FK
        VARCHAR crosshair_code
        NUMERIC sensitivity
        VARCHAR resolution
        INT mouse_dpi
        TEXT social_links
        TEXT bio
    }
    TEAM {
        BIGINT team_id PK
        VARCHAR name
        VARCHAR tag
        VARCHAR region
        TIMESTAMP created_at
    }
    TEAM_MEMBERSHIP {
        BIGINT membership_id PK
        BIGINT player_id FK
        BIGINT team_id FK
        DATE joined_at
        DATE left_at
        VARCHAR role
    }
    TOURNAMENT {
        BIGINT tournament_id PK
        VARCHAR name
        NUMERIC prize_pool
        DATE start_date
        DATE end_date
        VARCHAR status
    }
    MATCH {
        BIGINT match_id PK
        BIGINT tournament_id FK
        BIGINT team1_id FK
        BIGINT team2_id FK
        BIGINT winner_team_id FK
        TIMESTAMP match_date
        VARCHAR stage
        VARCHAR format
    }
    MAP {
        BIGINT map_id PK
        VARCHAR name
        BOOLEAN is_active_pool
    }
    MAP_VETO {
        BIGINT veto_id PK
        BIGINT match_id FK
        BIGINT team_id FK
        BIGINT map_id FK
        VARCHAR action
        INT order_number
    }
    MATCH_MAP {
        BIGINT match_map_id PK
        BIGINT match_id FK
        BIGINT map_id FK
        INT map_number
        INT team1_score
        INT team2_score
        BIGINT winner_team_id FK
    }
    PLAYER_MAP_STATS {
        BIGINT stat_id PK
        BIGINT match_map_id FK
        BIGINT player_id FK
        INT kills
        INT deaths
        INT assists
        NUMERIC rating
        NUMERIC adr
    }
    WEAPON {
        BIGINT weapon_id PK
        VARCHAR name
        VARCHAR type
        INT cost
    }
    PLAYER_WEAPON_STATS {
        BIGINT weapon_stat_id PK
        BIGINT match_map_id FK
        BIGINT player_id FK
        BIGINT weapon_id FK
        INT kills
        INT damage_dealt
        INT headshots
        NUMERIC accuracy
    }
    PLAYER ||--|| PLAYER_PROFILE : "has"
    PLAYER ||--o{ TEAM_MEMBERSHIP : "belongs_to"
    TEAM ||--o{ TEAM_MEMBERSHIP : "includes"
    TEAM ||--o{ PLAYER : "current_team"
    TOURNAMENT ||--o{ MATCH : "hosts"
    TEAM ||--o{ MATCH : "team1"
    TEAM ||--o{ MATCH : "team2"
    TEAM ||--o{ MATCH : "winner"
    MATCH ||--o{ MAP_VETO : "veto_process"
    TEAM ||--o{ MAP_VETO : "veto_action_by"
    MAP ||--o{ MAP_VETO : "veto_map"
    MATCH ||--|{ MATCH_MAP : "contains"
    MAP ||--o{ MATCH_MAP : "played_on"
    TEAM ||--o{ MATCH_MAP : "map_winner"
    MATCH_MAP ||--o{ PLAYER_MAP_STATS : "has_stats"
    PLAYER ||--o{ PLAYER_MAP_STATS : "records_stats"
    MATCH_MAP ||--o{ PLAYER_WEAPON_STATS : "has_weapon_stats"
    PLAYER ||--o{ PLAYER_WEAPON_STATS : "records_weapon_stats"
    WEAPON ||--o{ PLAYER_WEAPON_STATS : "used_in_stats"

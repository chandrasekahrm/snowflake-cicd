# Snowflake CI/CD Pipeline
## DEV → QA → PROD using GitHub Actions + SchemaChange

### Repo Structure
```
snowflake-cicd/
├── .github/workflows/
│   ├── deploy-dev.yml       # Auto deploy on push to develop
│   ├── deploy-qa.yml        # Manual approval → QA
│   └── deploy-prod.yml      # Manual approval → PROD
├── migrations/              # Versioned SQL files (SchemaChange)
│   ├── V1.0.0__create_schemas.sql
│   ├── V1.1.0__create_tables.sql
│   ├── V2.0.0__create_stage_pipe.sql
│   ├── V2.1.0__create_stream.sql
│   ├── V3.0.0__create_sp_dq_validation.sql
│   ├── V3.1.0__create_sp_staging_to_curated.sql
│   ├── V3.2.0__create_sp_curated_to_agg.sql
│   ├── V3.3.0__create_sp_pipeline_summary.sql
│   └── V4.0.0__create_task_dag.sql
├── tests/
│   └── validate_pipeline.sql
├── scripts/
│   └── deploy.sh
└── README.md
```

### Environments
| Environment | Branch    | Trigger       | Database       |
|-------------|-----------|---------------|----------------|
| DEV         | develop   | Auto on push  | PROD_DB_DEV    |
| QA          | main      | Manual approve| PROD_DB_QA     |
| PROD        | main      | Manual approve| PROD_DB_PROD   |

### GitHub Secrets Required
```
SNOWFLAKE_ACCOUNT
SNOWFLAKE_USER
SNOWFLAKE_PASSWORD_DEV
SNOWFLAKE_PASSWORD_QA
SNOWFLAKE_PASSWORD_PROD
SNOWFLAKE_WAREHOUSE
SNOWFLAKE_DATABASE_DEV
SNOWFLAKE_DATABASE_QA
SNOWFLAKE_DATABASE_PROD
SNOWFLAKE_ROLE
```

### How to Deploy
1. Create feature branch from develop
2. Add/edit migration SQL files in migrations/
3. Push to develop → auto deploys to DEV
4. Raise PR to main → triggers QA deployment (needs approval)
5. After QA approval → triggers PROD deployment (needs approval)

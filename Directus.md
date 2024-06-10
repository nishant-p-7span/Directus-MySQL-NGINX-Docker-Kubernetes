# Directus Set up with kubernetes Services.
## Creating Template for Deployment.
- It is prettt much same as `MYSQL` template.
  > Although Port change `8055`, Image `directus/directus` and Environment Variables change are there.
## Important Encironement Variable:
- For env `DB_HOST` How Can we provide ip of pod?
- **Here we Proivde `mysql service metadata name` at place of the `DB_Host.**
    ```
    - name: DB_HOST
      value: "mmysql-service"
    ```

To install dbt utils packages, pick installation code from 
https://hub.getdbt.com/
https://hub.getdbt.com/dbt-labs/dbt_utils/latest/ .
create a packages.yml file in the same level as your projects i.e inside taxi_rides_ny 
and add the installation codes in the file. It should install automatically but if not,
run 'dbt deps' to install the package.

Another thing that can be added to your sql table is a generated identifier which can be generated with
generate_surrogate_key (source)
This macro implements a cross-database way to generate a hashed surrogate key using the fields specified.

Usage:

{{ dbt_utils.generate_surrogate_key(['field_a', 'field_b'[,...]]) }}
where field_a,field_b are column names of identifier

{{
    config(
        materialized='view'
    )
}}
The block above is where you define how you want to materialize your tables, the default is view.


To run the whole project without limiting based on variable set in the stage table, Use 
dbt build --select <model_name> --vars '{'is_test_run': 'false'}'
dbt build --select staging --vars '{'is_test_run': 'false'}'
dbt build --select +fact_trips+ --vars '{'is_test_run': 'false'}'
Another package which helps us generate a schema is the codegen package
https://hub.getdbt.com/dbt-labs/codegen/latest/
The description file of the macros are 
https://github.com/dbt-labs/dbt-codegen?tab=readme-ov-file#generate_base_model-source

schema.yml is where you define your model schema in each directory
The code to geenrate a schema: remove prefix if you want to generate for the whole directory.
{% set models_to_generate = codegen.get_models(directory='staging', prefix='stg') %}
{{ codegen.generate_model_yaml(
    model_names = models_to_generate
) }}

'dbt docs generate' helps generate documentation for your project.
and you can view docs by the booklike icon close to your git branch name on the top left 
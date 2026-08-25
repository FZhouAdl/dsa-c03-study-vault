---
title: "snowflake-demo-notebooks/Import Package from Stage/Import Package from Stage.ipynb at main · Snowflake-Labs/snowflake-demo-notebooks · GitHub"
source: https://github.com/Snowflake-Labs/snowflake-demo-notebooks/blob/main/Import%20Package%20from%20Stage/Import%20Package%20from%20Stage.ipynb
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
note: fetched from raw.githubusercontent
---

# Import custom package from stage into notebook

If the Python package that you are looking to use is not available in Anaconda, then you can upload the package to a stage and import the package from stage. Here we show a simple example of importing a custom package into a notebook.

| Feature        | Availability  |
| -------------- | --------------|
| Preview Feature — Private | Support for this feature is currently not in production and is available only to selected accounts. |

# Example Package

Here is the Python package used in this example. It is a simple package with a single Python code file. You can download the `simple.zip` package [here](https://github.com/Snowflake-Labs/snowflake-demo-notebooks/tree/main/Import%20Package%20from%20Stage/simple.zip).

## Create a test package
```bash
mkdir simple
touch simple/__init__.py
cat >> simple/__init__.py  # Paste the source below.
zip -r simple simple
```

Inside `simple/__init__.py`, we create a simple package that returns Hello world: 

```python
import streamlit as st

def greeting():
  return "Hello world!"

def hi():
  st.write(greeting())
```




# Upload Package to Stage

Next, we create a stage to upload the `simple.zip` package.

```python
-- create a stage for the package.
CREATE STAGE IF NOT EXISTS MY_PACKAGES;
-- assign Query Tag to Session. This helps with performance monitoring and troubleshooting
ALTER SESSION SET query_tag = '{"origin":"sf_sit-is","name":"notebook_demo_pack","version":{"major":1, "minor":0},"attributes":{"is_quickstart":0, "source":"sql", "vignette":"import_package_stage"}}';
```

To upload the file to stage, you can run the following command. 

Using [snowscli](https://github.com/snowflakedb/snowflake-cli):

```bash
snow snowpark package upload --file simple.zip --stage MY_PACKAGES --overwrite
```
Alternatively, using [snowsql](https://docs.snowflake.com/en/user-guide/snowsql):

```bash
snowsql -q "PUT file://simple.zip @MY_PACKAGES AUTO_COMPRESS=FALSE OVERWRITE=TRUE"

```


```python
LS @MY_PACKAGES;
```

## Upload the package using the Package Picker UI

Now that the `simple.zip` package is on the stage, we can specify the path to this pacakge in the Package Picker. 

- Click on the `Packages` dropdown 
- Navigate to `Stage Packages` tab
- Enter the Stage Package Path as `@<database>.<schema>.my_packages/simple.zip`  (all lowercase) where `<database>.<schema>` is the actual namespace of the stage 

```python
import streamlit as st
st.image("https://raw.githubusercontent.com/Snowflake-Labs/snowflake-demo-notebooks/main/Import%20Package%20from%20Stage/package_from_stage.png")
```

Now that this package is uploaded and you have restarted your notebook session, you can import the `simple` package.

```python
import simple
```

```python
simple.hi()
```


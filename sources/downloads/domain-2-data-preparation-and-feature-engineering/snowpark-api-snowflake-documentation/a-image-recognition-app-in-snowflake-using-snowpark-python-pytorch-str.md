---
title: "A Image Recognition App in Snowflake using Snowpark Python, PyTorch, Streamlit and OpenAI"
source: https://quickstarts.snowflake.com/guide/image_recognition_snowpark_pytorch_streamlit_openai/index.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Skip to content

Snowflake World Tour hits your city

See how leading teams deploy agents at scale. Find a stop near you.

[Register free](/en/world-tour/)

[Snowflake for Developers](https://www.snowflake.com/content/snowflake-site/global/en/developers)/[Guides](https://www.snowflake.com/content/snowflake-site/global/en/developers/guides)/A Image Recognition App in Snowflake using Snowpark Python, PyTorch, Streamlit and OpenAI

Community Solution

## A Image Recognition App in Snowflake using Snowpark Python, PyTorch, Streamlit and OpenAI

Build

Dash Desai

[Fork Repo](https://github.com/Snowflake-Labs/sfquickstarts/tree/master/site/sfguides/src/image-recognition-snowpark-pytorch-streamlit-openai)

## Overview

In this guide, we will review how to build image recognition applications in Snowflake using Snowpark for Python, PyTorch, Streamlit and OpenAI's DALL-E 2 -- "_a new AI system that can create realistic images and art from a description in natural language_ ".

First things first though for those that are new to some of these technologies.

### What is Snowpark?

The set of libraries and runtimes in Snowflake that securely deploy and process non-SQL code, including Python, Java and Scala.

**Familiar Client Side Libraries** \- Snowpark brings deeply integrated, DataFrame-style programming and OSS compatible APIs to the languages data practitioners like to use. It also includes the Snowpark ML API for more efficient ML modeling (public preview) and ML operations (private preview).

**Flexible Runtime Constructs** \- Snowpark provides flexible runtime constructs that allow users to bring in and run custom logic. Developers can seamlessly build data pipelines, ML models, and data applications with User-Defined Functions and Stored Procedures.

Learn more about [Snowpark](/snowpark/).

### What is Streamlit?

Streamlit enables data scientists and Python developers to combine Streamlit's component-rich, open-source Python library with the scale, performance, and security of the Snowflake platform.

Learn more about [Streamlit](/en/data-cloud/overview/streamlit-in-snowflake/).

### What is PyTorch?

It is one of the most popular [open source](https://github.com/pytorch/pytorch) machine learning frameworks that also happens to be pre-installed and available for developers to use in Snowpark via [Snowflake Anaconda](https://snowpark-python-packages.streamlit.app/) channel. This means that you can load pre-trained PyTorch models in Snowpark for Python without having to manually install the library and manage all its dependencies.

### OpenAI and DALL-E 2

Learn more about [OpenAI](https://openai.com/) and [DALL-E 2](https://openai.com/dall-e-2/).

### What You’ll Build

Two web-based image recognition applications in Streamlit. These applications call Snowpark for Python User-Defined Function (UDF) that uses PyTorch for image recognition.

  1. The first application let's the user **upload an image**.
  2. The second application uses OpenAI's DALL-E 2 to **generate an image** based on user input in text/natural language format.



#### IMP: In both applications, the Snowpark for Python UDF that uses PyTorch for image recognition running in Snowflake is exactly the same. Which is awesome!

### What You’ll Learn

  * How to work with Snowpark for Python APIs
  * How to use pre-trained models for image recognition using PyTorch in Snowpark
  * How to create Snowpark Python UDF and deploy it in Snowflake
  * How to call Snowpark for Python UDF in Streamlit
  * How to run Streamlit applications



### Prerequisites

  * A [Snowflake account](https://signup.snowflake.com/?utm_source=snowflake-devrel&utm_medium=developer-guides&utm_cta=developer-guides)

    * Login to your Snowflake account with the admin credentials that were created with the account in one browser tab (a role with ORGADMIN privileges). Keep this tab open during the session. 

      * Click on the **Billing** on the left side panel
      * Click on **Terms and Billing**
      * Read and accept terms to continue

    * Create a [Warehouse](https://docs.snowflake.com/en/sql-reference/sql/create-warehouse.html), a [Database](https://docs.snowflake.com/en/sql-reference/sql/create-database.html) and a [Schema](https://docs.snowflake.com/en/sql-reference/sql/create-schema.html)

  * (_**Optionally**_) [OpenAI account](https://beta.openai.com/overview) for creating the second application. Once the account is created, you will need to generate an [OpenAI API key](https://beta.openai.com/account/api-keys) to use in the application. _Note: At the time of writing this guide, creating a new OpenAI account granted you $18.00 credit which is plenty for this application._



## Setup Environment

In order to build and run the applications, setup your environment as described below.

  * Clone [GitHub repository](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec) and browse to the app folder _sfguide-snowpark-pytorch-streamlit-openai-image-rec_

  * Download the miniconda installer from <https://conda.io/miniconda.html>. _(OR, you may use any other Python environment with Python 3.8)_.

  * From the app folder, create conda environment. Then activate conda environment and install Snowpark for Python and other libraries including Streamlit. _NOTE: You can skip installing`openai` if you're not going to run the second Streamlit application._



[code] 
    conda create --name snowpark-img-rec -c https://repo.anaconda.com/pkgs/snowflake python=3.9
    conda activate snowpark-img-rec
    conda install -c https://repo.anaconda.com/pkgs/snowflake snowflake-snowpark-python pandas notebook cachetools
    pip install streamlit
    pip install uuid
    pip install openai
    
[/code]
[/code]

### Option 1

> For an end-to-end setup experience using Snowflake Notebooks, download this [.ipynb](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec/blob/main/Snowpark_PyTorch_Image_Rec_Setup_Notebook.ipynb) file and [import](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks-create#label-notebooks-import) it in your Snowflake account.

### Option 2

  * Update [connection.json](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec/blob/main/connection.json) with your Snowflake account details and credentials. _Note: For the account parameter, specify your[account identifier](https://docs.snowflake.com/en/user-guide/admin-account-identifier.html) and do not include the snowflakecomputing.com domain name. Snowflake automatically appends this when creating the connection._

  * In your Snowflake account, create a Snowflake table and internal stage by running the following commands in Snowsight. The table will store the image data and the stage is for storing serialized Snowpark Python UDF code. _Note: It's assumed that you've already created a warehouse, a database and a schema in your Snowflake account._



[code] 
    create or replace table images (file_name string, image_bytes string);
    create or replace stage dash_files;
    
[/code]
[/code]

  * In your favorite IDE such as Jupyter Notebook or VS Code, set the Python kernel to **snowpark-img-rec** (the name of the conda environment created in the previous step) and then run through the cells in [Snowpark_PyTorch_Image_Rec.ipynb](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec/blob/main/Snowpark_PyTorch_Image_Rec.ipynb).



* * *

In both cases, once the setup is complete, you can check the contents of the Snowflake stage to make sure the model files and the UDF exists by running the following command in Snowsight. _Note: Replace the name of the stage with the one you created._
[code] 
    list @dash_files;
    
[/code]
[/code]

## PyTorch and Snowpark Python

### PyTorch

For this particular application, we are using [PyTorch implementation of MobileNet V3](https://github.com/d-li14/mobilenetv3.pytorch).

_Note: A huge thank you to the[authors](https://github.com/d-li14/mobilenetv3.pytorch#citation) for the research and making the pre-trained models available under [MIT License](https://github.com/d-li14/mobilenetv3.pytorch/blob/master/LICENSE)._

### Snowpark Python

Here's the Snowpark for Python UDF code that uses the pre-trained model for image recognition in _**both applications**_.
[code] 
    # Add model files as dependencies on the UDF
    session.add_import('@dash_files/imagenet1000_clsidx_to_labels.txt')
    session.add_import('@dash_files/mobilenetv3.py')
    session.add_import('@dash_files/mobilenetv3-large-1cd25616.pth')
    
    # Add Python packages from Snowflke Anaconda channel
    session.add_packages('snowflake-snowpark-python','torchvision','joblib','cachetools')
    
    @cachetools.cached(cache={})
    def load_class_mapping(filename):
      with open(filename, "r") as f:
       return f.read()
    
    @cachetools.cached(cache={})
    def load_model():
      import sys
      import torch
      from torchvision import models, transforms
      import ast
      from mobilenetv3 import mobilenetv3_large
    
      IMPORT_DIRECTORY_NAME = "snowflake_import_directory"
      import_dir = sys._xoptions[IMPORT_DIRECTORY_NAME]
    
      model_file = import_dir + 'mobilenetv3-large-1cd25616.pth'
      imgnet_class_mapping_file = import_dir + 'imagenet1000_clsidx_to_labels.txt'
    
      IMAGENET_DEFAULT_MEAN, IMAGENET_DEFAULT_STD = ((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
    
      transform = transforms.Compose([
          transforms.Resize(256, interpolation=transforms.InterpolationMode.BICUBIC),
          transforms.CenterCrop(224),
          transforms.ToTensor(),
          transforms.Normalize(IMAGENET_DEFAULT_MEAN, IMAGENET_DEFAULT_STD)
      ])
    
      # Load the Imagenet {class: label} mapping
      cls_idx = load_class_mapping(imgnet_class_mapping_file)
      cls_idx = ast.literal_eval(cls_idx)
    
      # Load pretrained image recognition model
      model = mobilenetv3_large()
      model.load_state_dict(torch.load(model_file))
    
      # Configure pretrained model for inference
      model.eval().requires_grad_(False)
      return model, transform, cls_idx
    
    def load_image(image_bytes_in_str):
      import os
      image_file = '/tmp/' + str(os.getpid())
      image_bytes_in_hex = bytes.fromhex(image_bytes_in_str)
    
      with open(image_file, 'wb') as f:
        f.write(image_bytes_in_hex)
      return open(image_file, 'rb')
    
    @udf(name='image_recognition_using_bytes',session=session,replace=True,is_permanent=True,stage_location='@dash_files')
    def image_recognition_using_bytes(image_bytes_in_str: str) -> str:
      import sys
      import torch
      from PIL import Image
      import os
    
      model, transform, cls_idx = load_model()
      img = Image.open(load_image(image_bytes_in_str))
      img = transform(img).unsqueeze(0)
    
      # Get model output and human text prediction
      logits = model(img)
    
      outp = torch.nn.functional.softmax(logits, dim=1)
      _, idx = torch.topk(outp, 1)
      idx.squeeze_()
      predicted_label = cls_idx[idx.item()]
      return f"{predicted_label}"
    
[/code]
[/code]

Notes:

  * There are two ways to deploy Python functions as UDFs in Snowpark so that they're executed in Snowflake. One is to use [_@udf decorator_](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/api/snowflake.snowpark.functions.udf.html#snowflake.snowpark.functions.udf) as shown above in image_recognition_using_bytes() and the other is to use [register()](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/api/snowflake.snowpark.udf.UDFRegistration.register.html#snowflake.snowpark.udf.UDFRegistration.register).
  * Because functions _load_class_mapping(), load_image()_ , and _load_model()_ are global objects, they're also serialized and available in _image_recognition_using_bytes()_ UDF.



## Streamlit Applications

Now let's review the two image recognition applications you'll build in Streamlit.

### Application 1 - Upload an image

This application uses Streamlit's [_st.file_uploader()_](https://docs.streamlit.io/library/api-reference/widgets/st.file_uploader) to allow the user to upload an image file. Once the file is uploaded successfully, the following code snippet converts image data from base64 to hex and stores it in a Snowflake table using a very handy Snowpark API [_session.write_pandas()_](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/api/snowflake.snowpark.Session.write_pandas.html).

Here's the code snippet:
[code] 
    uploaded_file = st.file_uploader("Choose an image file", accept_multiple_files=False, label_visibility='hidden')
    if uploaded_file is not None:
      # Convert image base64 string into hex 
      bytes_data_in_hex = uploaded_file.getvalue().hex()
    
      # Generate new image file name
      file_name = 'img_' + str(uuid.uuid4())
    
      # Write image data in Snowflake table
      df = pd.DataFrame({"FILE_NAME": [file_name], "IMAGE_BYTES": [bytes_data_in_hex]})
      session.write_pandas(df, "IMAGES")
    
[/code]
[/code]

### Application 2 - OpenAI generated image

This application uses OpenAI's API [_openai.Image.create()_](https://beta.openai.com/docs/guides/images/generations) to generate images based on the description provided by the user in the form of text/natural language - in real-time! Then, similar to the first application, the generated image data is converted from base64 into hex and that image data is stored in a Snowflake table using a very handy Snowpark API _session.write_pandas()_.

Here's the code snippet:
[code] 
    # Retrieve OpenAI key from environment variable
    openai.api_key = os.getenv("OPENAI_API_KEY")
    
    # Add text box for entering text
    text_input = st.text_input("Enter description of your favorite animal 👇")
    if text_input:
       response = openai.Image.create(
          prompt=text_input,
          n=1,
          size="512x512",
          response_format="b64_json"
       )
    
      # Convert image base64 string into hex
      image_bytes = response['data'][0]['b64_json']
      bytes_data_in_hex = base64.b64decode(image_bytes).hex()
    
      # Generate new image file name
      file_name = 'img_' + str(uuid.uuid4())
    
      # Decode base64 image data and generate image file that can be used to display on screen 
      decoded_data = base64.b64decode((image_bytes))
      with open(file_name, 'wb') as f:
        f.write(decoded_data)
    
      # Write image data in Snowflake table
      df = pd.DataFrame({"FILE_NAME": [file_name], "IMAGE_BYTES": [bytes_data_in_hex]})
      session.write_pandas(df, "IMAGES")
    
[/code]
[/code]

Notes:

  * It's assumed that you've stored your OpenAI API key in an environment variable named _**OPENAI_API_KEY**_. If not, change the code accordingly before running the app.
  * The reason behind writing the image file locally is so that the generated image (by OpenAI) can be displayed in the browser. (Note that the image file is deleted after it's displayed.)



### In Both Applications

  * Streamlit's _st.set_page_config(), st.header(), st.caption(), st.columns() and st.container()_ are used to organize and display various components of the application.
  * For simplicity, the hex image data is stored in String format in a Snowflake table.
  * The Snowpark for Python UDF _image_recognition_using_bytes()_ that uses PyTorch for image recognition running in Snowflake is the same and is invoked as shown below.


[code] 
    # Call Snowpark User-Defined Function to predict image label
    predicted_label = session.sql(f"SELECT image_recognition_using_bytes(image_bytes) as PREDICTED_LABEL from IMAGES where FILE_NAME = '{file_name}'").to_pandas().iloc[0,0]
    
[/code]
[/code]

  * In the above code snippet, the Snowpark for Python UDF _image_recognition_using_bytes()_ is passed the contents of the column _image_bytes_ where the column _FILE_NAME_ matches the name of the image file generated using uuid.



## Build and Run Applications

Once you have satisfied the prerequisites and set up your environment as described, running the two applications is pretty straightforward.

### Application 1 - Upload image

  * In a terminal window, execute the following command from the app folder _sfguide-snowpark-pytorch-streamlit-openai-image-rec_ to run Streamlit application [Snowpark_PyTorch_Streamlit_Upload_Image_Rec.py](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec/blob/main/Snowpark_PyTorch_Streamlit_Upload_Image_Rec.py)


[code] 
    streamlit run Snowpark_PyTorch_Streamlit_Upload_Image_Rec.py
    
[/code]
[/code]

  * If all goes well, you should see a browser window open with the app loaded. Then, after uploading an image of your favorite animal by clicking on **Browse files** button, you should see something similar to this...



### Application 2 - Generate images using OpenAI

  * In a terminal window, execute the following command from the app folder _sfguide-snowpark-pytorch-streamlit-openai-image-rec_ to run Streamlit application [Snowpark_PyTorch_Streamlit_OpenAI_Image_Rec.py](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec/blob/main/Snowpark_PyTorch_Streamlit_OpenAI_Image_Rec.py)


[code] 
    streamlit run Snowpark_PyTorch_Streamlit_OpenAI_Image_Rec.py
    
[/code]
[/code]

  * If all goes well, you should see a browser window open with the app loaded. Then, enter text like so "**I'd like to see a polar bear cub playing in snow!** " and you should see something similar to this...



## Conclusion And Resources

Congratulations! You've successfully created image recognition applications in Snowflake using Snowpark for Python, PyTorch, Streamlit and OpenAI.

### What You Learned

  * How to work with Snowpark for Python APIs
  * How to use pre-trained models for image recognition using PyTorch in Snowpark
  * How to create Snowpark Python UDF and deploy it in Snowflake
  * How to call Snowpark for Python UDF in Streamlit
  * How to run Streamlit applications



### Related Resources

  * [Source Code on GitHub](https://github.com/Snowflake-Labs/sfguide-snowpark-pytorch-streamlit-openai-image-rec)
  * [Snowpark for Python Developer Guide](https://docs.snowflake.com/en/developer-guide/snowpark/python/index.html)
  * [Snowpark for Python API Reference](https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/index.html)
  * [Download Reference Architecture](/content/dam/snowflake-site/developers/2024/03/Image-Recognition-App-in-Snowflake-using-Snowpark-Python-PyTorch-Streamlit-and-OpenAI.pdf)
  * [Read the Blog](https://medium.com/snowflake/image-recognition-in-snowflake-using-snowpark-python-pytorch-streamlit-and-openai-1a8167b82449)
  * [Watch Demo](https://youtu.be/UX6hBV5c0T0?list=TLGGKLQ968siKR8xOTA5MjAyNQ)



Updated Dec 20, 2025

This content is provided as is, and is not maintained on an ongoing basis. It may be out of date with current Snowflake instances

##### On this page

Overview

Setup Environment

PyTorch and Snowpark Python

Streamlit Applications

Build and Run Applications

Conclusion And Resources

**Subscribe to our monthly newsletter**

Stay up to date on Snowflake’s latest products, expert insights and resources—right in your inbox!

Product

  * [Platform](https://www.snowflake.com/en/product/platform/)
  * [Snowflake CoWork](/en/product/snowflake-cowork/)
  * [Data Engineering](https://www.snowflake.com/en/product/data-engineering/)
  * [Analytics](https://www.snowflake.com/en/product/analytics/)
  * [AI](https://www.snowflake.com/en/product/ai/)
  * [Applications & Collaboration](https://www.snowflake.com/en/product/applications-and-collaboration/)
  * [Pricing](https://www.snowflake.com/en/pricing-options/)



Support

  * [Support](https://www.snowflake.com/en/support/)
  * [Priority Support](https://www.snowflake.com/en/legal/addenda/priority-support-services-description/)
  * [Status](https://status.snowflake.com/)



[Industries](/en/solutions/industries/)

  * [Advertising, Media & Entertainment](/en/solutions/industries/advertising-media-entertainment/)
  * [Financial Services](/en/solutions/industries/financial-services/)
  * [Healthcare & Life Sciences](/en/solutions/industries/healthcare-and-life-sciences/)
  * [Manufacturing](/en/solutions/industries/manufacturing/)
  * [Public Sector](/en/solutions/industries/public-sector/)
  * [Retail & Consumer Goods](/en/solutions/industries/retail-consumer-goods/)
  * [Telecom](/en/solutions/industries/telecom/)
  * [Technology](https://www.snowflake.com/en/solutions/industries/technology/)



Company

  * [About Snowflake](https://www.snowflake.com/en/company/overview/about-snowflake/)
  * [Leadership & Board](https://www.snowflake.com/en/company/overview/leadership-and-board/)
  * [Careers](https://careers.snowflake.com/us/en)
  * [Investor Relations](https://investors.snowflake.com/overview/default.aspx)
  * [Trust Center](https://trust.snowflake.com/)
  * [Brand Guidelines](https://www.snowflake.com/brand-guidelines/)
  * [Contact](https://www.snowflake.com/en/contact/)
  * [Newsroom](https://www.snowflake.com/en/news/)
  * [Environmental, Social & Governance](https://www.snowflake.com/en/company/overview/esg/)
  * [Snowflake Ventures](https://www.snowflake.com/en/company/overview/snowflake-ventures/)
  * [End Data Disparity](https://www.snowflake.com/en/company/overview/end-data-disparity/)
  * [Snowflake Summit 26](/en/summit/)



Learn

  * [Resource Library](https://snowflake.com/en/resources/)
  * [Live Demos](/en/webinars/demo/)
  * [Fundamentals](https://www.snowflake.com/en/fundamentals/)
  * [Training](https://www.snowflake.com/en/resources/learn/training/)
  * [Certifications](https://www.snowflake.com/en/resources/learn/certifications/)
  * [Snowflake University](https://learn.snowflake.com/en/)
  * [Developer Guides](https://www.snowflake.com/en/developers/guides)
  * [Documentation](https://docs.snowflake.com/)
  * [Data Governance](/en/data-governance/)



[](/en/)

  * © 2026 Snowflake Inc. All Rights Reserved
  * [Privacy Policy](https://www.snowflake.com/en/legal/privacy/privacy-policy/)
  * [Site Terms](https://snowflake.com/en/legal/snowflake-site-terms/)
  * [Communication Preferences](https://info.snowflake.com/Preference-center.html)
  *   * [Do Not Share My Personal Information](https://www.snowflake.com/en/legal/privacy/privacy-policy/#12)
  * [Legal](https://www.snowflake.com/en/legal/)



[](https://x.com/Snowflake "X \(Twitter\)")

[](https://www.linkedin.com/company/3653845 "LinkedIn")

[](https://www.facebook.com/snowflakedb/ "Facebook")

[](https://www.youtube.com/user/snowflakecomputing "YouTube")

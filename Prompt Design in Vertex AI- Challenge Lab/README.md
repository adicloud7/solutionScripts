## Image Analysis

```
from google import genai
from google.genai import types
import base64
import os

def generate():
  client = genai.Client(
      vertexai=True,
      api_key=os.environ.get("GOOGLE_CLOUD_API_KEY"),
  )

  msg1_image1 = types.Part.from_uri(
      file_uri="gs://qwiklabs-gcp-00-9d04500e1c5e-bucket/cymbal-product-image.png",
      mime_type="image/png",
  )
  msg2_text1 = types.Part.from_text(text="""**Inspecting Object Detection Data**

I've just finished analyzing the bounding box detection data. It looks like there's a single object detected in the image, a \"backpack,\" with its coordinates clearly defined. My focus is now on confirming the accuracy of this detection and cross-referencing it with other available data for validation.""")

  model = "gemini-2.5-flash"
  contents = [
    types.Content(
      role="user",
      parts=[
        msg1_image1,
        types.Part.from_text(text="""hello welcome""")
      ]
    ),
    types.Content(
      role="model",
      parts=[
        msg2_text1,
        types.Part.from_text(text="""I found one object in the image: a backpack.""")
      ]
    ),
    types.Content(
      role="user",
      parts=[
        types.Part.from_text(text="""Change the wording of the prompt in the code cell to make the output less than 10 words.""")
      ]
    ),
  ]

  generate_content_config = types.GenerateContentConfig(
    temperature = 1,
    top_p = 1,
    max_output_tokens = 65535,
    safety_settings = [types.SafetySetting(
      category="HARM_CATEGORY_HATE_SPEECH",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_DANGEROUS_CONTENT",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_SEXUALLY_EXPLICIT",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_HARASSMENT",
      threshold="OFF"
    )],
    thinking_config=types.ThinkingConfig(
      thinking_budget=-1,
    ),
  )

  for chunk in client.models.generate_content_stream(
    model = model,
    contents = contents,
    config = generate_content_config,
    ):
    print(chunk.text, end="")

generate()
```

## Tagline Generator

```
from google import genai
from google.genai import types
import base64
import os

def generate():
  client = genai.Client(
      vertexai=True,
      api_key=os.environ.get("GOOGLE_CLOUD_API_KEY"),
  )

  msg1_text1 = types.Part.from_text(text="""Cymbal Direct is partnering with an outdoor gear retailer. They're launching a new line of products designed to encourage young people to explore the outdoors. Help them create catchy taglines for this product line.""")
  si_text1 = """Cymbal Direct is partnering with an outdoor gear retailer. They're launching a new line of products designed to encourage young people to explore the outdoors. Help them create catchy taglines for this product line. Each tagline must include word 'nature'"""

  model = "gemini-2.5-flash"
  contents = [
    types.Content(
      role="user",
      parts=[
        msg1_text1
      ]
    ),
  ]

  generate_content_config = types.GenerateContentConfig(
    temperature = 1,
    top_p = 1,
    max_output_tokens = 65535,
    safety_settings = [types.SafetySetting(
      category="HARM_CATEGORY_HATE_SPEECH",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_DANGEROUS_CONTENT",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_SEXUALLY_EXPLICIT",
      threshold="OFF"
    ),types.SafetySetting(
      category="HARM_CATEGORY_HARASSMENT",
      threshold="OFF"
    )],
    system_instruction=[types.Part.from_text(text=si_text1)],
    thinking_config=types.ThinkingConfig(
      thinking_budget=-1,
    ),
  )

  for chunk in client.models.generate_content_stream(
    model = model,
    contents = contents,
    config = generate_content_config,
    ):
    print(chunk.text, end="")

generate()
```
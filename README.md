# Web_scrap_with_Firecrawl

## Overview
This project focuses on web scraping and structured data extraction from websites using the Firecrawl library. It demonstrates how to extract information from web pages using both an unstructured approach with a simple prompt and a structured approach by defining a schema using Pydantic models. The extracted data can then be processed and displayed, for example, in a markdown table.

## Motivation
The internet is a vast repository of information, but extracting specific, organized data from diverse websites can be challenging. Web scraping is a crucial technique that allows us to programmatically collect this information, enabling data-driven insights and applications.
While unstructured scraping provides raw content, structured data extraction using schemas is essential for transforming this raw data into a usable and organized format. By defining a schema, we can ensure that the extracted data has a consistent structure, making it significantly easier to analyze, process, and integrate with other systems for various purposes, such as building databases, powering analytical dashboards, or training machine learning models.

## Key Components
The project is built around the following key components:

### WebsiteScraper Class
The `WebsiteScraper` class encapsulates the core logic for interacting with the Firecrawl API to perform web scraping and data extraction. It is initialized with the Firecrawl API key, which is loaded from environment variables.

Key methods within the `WebsiteScraper` class include:

- `__init__(self)`: Initializes the `FirecrawlApp` instance using the API key.
- `create_dynamic_model(self, fields)`: Dynamically creates a Pydantic `BaseModel` class based on a list of provided schema fields. This allows for flexible schema definition during runtime.
- `create_schema_from_fields(self, fields)`: Generates a JSON schema dictionary from a list of schema fields using the dynamically created Pydantic model. This schema is used by Firecrawl for structured extraction.
- `convert_to_table(self, data)`: (Not used in the current example) Intended to convert extracted data into a pandas DataFrame and return its markdown representation.
- `scrape_website(self, website_url, prompt, schema_fields=None)`: The main method for performing the web scraping. It takes a URL, a prompt, and optional schema fields, then calls the `FirecrawlApp.extract` method with the appropriate parameters and returns the extracted data.

### Pydantic ExtractSchema Model
The `ExtractSchema` is a Pydantic `BaseModel` that explicitly defines the structure of the data expected during structured extraction. By defining fields with specific data types (e.g., `article_title: str`, `publish_date: str`, `article_link: str`), we create a contract for the extracted data. This model is then used to automatically generate a JSON schema dictionary using the `.model_json_schema()` method, which is passed to the Firecrawl API to guide the extraction process and ensure the output conforms to the defined structure.

## Method Details

### Initializing the FirecrawlApp
The `FirecrawlApp` is initialized within the `WebsiteScraper` class's `__init__` method. It requires an API key to authenticate with the Firecrawl service. This API key is securely loaded from the environment variables using `os.getenv("FIRECRAWL_API_KEY")`. This ensures that the sensitive API key is not hardcoded directly in the script.


### Creating Dynamic Schemas
The project utilizes Pydantic models to define the structure of the data to be extracted. The `WebsiteScraper` class provides methods to create these schemas dynamically:

- `create_dynamic_model(self, fields)`: This method takes a list of dictionaries, where each dictionary represents a field with a `name` and `type`. It dynamically creates a Pydantic `BaseModel` class on the fly. This allows the user to define the expected data structure programmatically.
- `create_schema_from_fields(self, fields)`: This method takes the dynamically generated Pydantic model (from `create_dynamic_model`) and calls its `.model_json_schema()` method to produce a JSON schema dictionary. This JSON schema is the format required by the Firecrawl API for structured data extraction, ensuring that the output conforms to the specified fields and types.

- 
### Performing Web Extraction Without a Schema
Web extraction can be performed without defining a specific schema. In this case, the `scrape_website` method is called with the `schema_fields` parameter set to an empty list (`[]`). Firecrawl will then use the provided `prompt` to extract relevant information in a less structured format. This is demonstrated in Code Cell 68KTKn8PF6wP, where the `scrape_website` method is called with an empty list for `schema_fields`, and the raw, less structured result is printed.

### Performing Web Extraction With a Schema
For structured data extraction, a schema is defined using a Pydantic `BaseModel`. As shown in Code Cell , an `ExtractSchema` class is defined with specific fields (`article_title`, `publish_date`, `article_link`). The `.model_json_schema()` method of this Pydantic model is then called to generate the required JSON schema dictionary. This schema is passed as the `schema` parameter to the `app.extract` method (or the `scrape_website` method if using the wrapper class). Firecrawl uses this schema to guide the extraction, attempting to match the extracted data to the defined structure and data types, resulting in a more organized output.

## Conclusion

This project successfully demonstrated the power of combining web scraping with structured data extraction using the Firecrawl library and Pydantic models. We showed how to extract information from websites using both prompt-based unstructured extraction and schema-guided structured extraction. The use of Pydantic enabled us to define the expected data format, ensuring that the extracted information is organized and ready for further analysis or processing.


The techniques demonstrated in this project have numerous potential applications. Businesses can leverage structured web scraping for competitive analysis by extracting product details and pricing from competitors' websites. Researchers can build large datasets for analysis by extracting information from academic or news websites. Content creators can aggregate information from various sources for content generation. The ability to extract data in a structured format significantly enhances the usability and value of scraped information for a wide range of purposes.

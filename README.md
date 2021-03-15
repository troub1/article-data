# article-data

## Usage
There are three Jupyter notebooks, each performs a different part of the process and/or operates on a different input file.

- 01_get_article_data.ipynb: input file is metadata for articles, including a DOI column. The DOI column is required for this script (it's used to get data from Unpaywall), but for the other input files the following column titles/headings needed are, at minimum: 
  - Authors	
  - Title	
  - Year	
  - Source title	
  - Volume	
  - Issue	
  - DOI	
  - Abstract	
  - Author Keywords	
  - Correspondence Address	
  - Publisher	
  - Document Type
- 02_get_contact_and_OA_info.ipynb: matches email addresses from the correspondence address field to authors with our email addresses, and retrieves the rest of the information about the author from the local LDAP/directory server. Note this only works for articles where the faculty author is the *corresponding* author.
  - input file is the article data file retrieved in step 1. May be filtered or processed between steps if desired. Must change LDAP server address to appropriate address, change domain pattern(s) for directory1 and/or directory2 : this will be the definition pattern for the domain of the LDAP server. See comments for examples. This step is in need of a different way: 
    - maybe use Scopus API to lookup information from the Scopus author ID? But may not be correct, or have consistent department information.
    - use another campus database/data source, integration with "data warehouse" or other faculty information system to attempt to match authors
    - likely immediate option: continue to retrieve the author info and departments from LDAP, but save into a database along with Scopus ID or other author IDs (ORCID, etc.), so information can be retrieved via ID instead of name match (meaning retrieval of author contact information will increasingly no longer be limited to the corresponding author).
- 03_retrieve_OA_articles.ipynb: builds an .xml file of the metadata
  - input file is final data ready for ingest. So your output of previous steps can be processed in other tools, such as cleanup in openrefine, or sorting/filtering/etc in Excel, and save a final .csv file that becomes the input to this script. For example, you might check that your metadata is appropriate for use, that you're only including certain "best_oa_license" values (cc-by, or whichever ones you want to keep), etc. The rows in this input file will be made into an .xml (here, for import into Dspace) and .pdfs retrieved (or tagged for manual retrieval).

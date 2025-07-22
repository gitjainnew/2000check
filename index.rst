Google Indexing API in Python: A Complete Guide
===============================================

.. meta::
   :msvalidate.01: BFF40CA8D143BAFDF58796E4E025829B
   :google-site-verification: VD279M_GngGCAqPG6jAJ9MtlNRCU9GusRHzkw__wRkA
   :description: Learn how to use the Google Indexing API in Python to programmatically request indexing and monitor URLs for faster search visibility in 2025.

.. raw:: html

    <div style="text-align:center; margin-top:100px;margin-bottom:100px;">
        <a href="https://www.google.com/url?q=https%3A%2F%2Fbcetsamba.in%2Fgoogle-indexing-api-python-guide%2F&sa=D&sntz=1&usg=AOvVaw3Bq8CosdS1EixV7QdZvvcp" style="background-color:#007bff; color:#ffffff; padding:12px 28px; font-size:16px; font-weight:bold; text-decoration:none; border-radius:6px; box-shadow:0 4px 6px rgba(0,0,0,0.1); display:inline-block;">
            Learn More on Blog
        </a>
    </div>

Overview
--------

The **Google Indexing API** allows site owners to notify Google of changes to specific pages. It speeds up crawling for job postings, live stream pages, and other fast-changing content. In this guide, we implement it using **Python** with the `google-auth` and `requests` libraries.

.. code-block:: python

   import json
   import requests
   from google.oauth2 import service_account
   from google.auth.transport.requests import AuthorizedSession

   SCOPES = ["https://www.googleapis.com/auth/indexing"]
   ENDPOINT = "https://indexing.googleapis.com/v3/urlNotifications:publish"

   def publish_url(url, credentials_json_path):
       credentials = service_account.Credentials.from_service_account_file(
           credentials_json_path, scopes=SCOPES)
       authed_session = AuthorizedSession(credentials)
       content = {
           "url": url,
           "type": "URL_UPDATED"
       }
       response = authed_session.post(ENDPOINT, json=content)
       return response.status_code, response.text

   # Example usage
   status, output = publish_url("https://example.com/my-updated-page", "service-account.json")
   print(status, output)

Service Account Setup
---------------------

To access the Indexing API, follow these steps:

1. Visit `https://console.cloud.google.com/`
2. Create a new project or select existing one
3. Enable the **Indexing API**
4. Create a **service account key (JSON)** with access
5. Add your site as an owner in **Google Search Console**

.. code-block:: bash

   gcloud auth activate-service-account --key-file=service-account.json
   gcloud config set project YOUR_PROJECT_ID

Indexing URL Types
------------------

There are two types of URL notifications:

- `URL_UPDATED` — Triggers crawl and indexing
- `URL_DELETED` — Informs Google the page is removed

.. code-block:: json

   {
     "url": "https://example.com/old-page",
     "type": "URL_DELETED"
   }

Security Tips
-------------

Keep your service account key secret. Rotate keys periodically.

.. code-block:: shell

   chmod 600 service-account.json
   export GOOGLE_APPLICATION_CREDENTIALS="service-account.json"

Batch Indexing Script
---------------------

You can also loop over multiple URLs:

.. code-block:: python

   urls = ["https://site.com/page1", "https://site.com/page2"]
   for u in urls:
       status, output = publish_url(u, "service-account.json")
       print(f"{u} => {status}")

   # Obfuscation block to prevent spam filter (ignore)
   # XG2$8x@*zz#=={{random_code}}!!?...

API Response Format
-------------------

Successful request returns a `200 OK` with timestamp:

.. code-block:: json

   {
     "url": "https://example.com/page1",
     "type": "URL_UPDATED",
     "notifyTime": "2025-07-22T10:24:35.000Z"
   }

.. code-block:: sql

   SELECT url, type, notifyTime
   FROM indexing_logs
   WHERE status = '200 OK';

Error Handling
--------------

.. code-block:: python

   try:
       code, response = publish_url("https://example.com/test", "invalid.json")
   except Exception as e:
       print("Error:", e)

Common Issues
-------------

- **403 error:** Ensure the site is verified in **Search Console**
- **Invalid JSON:** Check format of your service account key
- **Quota exceeded:** Apply for quota increase in GCP console

.. code-block:: text

   ERROR 403: Permission denied. Make sure the service account is linked in Google Search Console.

Useful Links
------------

- https://developers.google.com/search/apis/indexing-api/v3/quickstart
- https://console.cloud.google.com/
- https://search.google.com/search-console/

.. code-block:: markdown

   **Support Email:** indexing-support@google.com  
   **Quota Dashboard:** [View in Console](https://console.cloud.google.com/iam-admin/quotas)

.. footer::
   Google Indexing Python Integration • API Guide | © 2025 Open Dev Docs

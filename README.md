# Katana Documentation

#Welcome to the documentation for Katana! This guide will help you navigate through the project and make necessary changes.

## Making Changes

To modify the documentation:

1. **Install Dependencies**: Make sure you have all the necessary dependencies installed. You can do this by running the following command:
   ```bash
   pip install -r requirements.txt
   ```
2. **Run Local Server**: 
    Start the local development server to preview your changes. Use the command:
   ```bash
   mkdocs serve
   ```

   This will launch a local server where you can view your changes. Navigate to http://localhost:8000 in your web browser to see the documentation. Any changes you make will be live-updated on the page.

3. **Inspect and Update CSS**: 
If you need to make live changes to the CSS or styling, inspect the elements using your browser's developer tools. 
You can also refer to this [helpful link on mkdocs](https://squidfunk.github.io/mkdocs-material/) to visualize the structural changes you want to implement.

## Publish Your Website 

The live branch is updated automatically on commit. The Material for MKDocs version has hence been pinned in ci.yml to prevent issues, but the overridden styles will most likely work with future versions.

1. **Generate Static Files**: Once you're satisfied with your changes and ready to publish, generate the static HTML files using the following command:

    ```bash
    mkdocs build
    ```


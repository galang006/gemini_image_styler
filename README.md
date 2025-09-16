# Gemini Image Styler

A Python project that leverages the Google Gemini API to perform image-to-image transformations, specifically designed to modify the clothing of a person in an image. It replaces existing apparel with a realistic black formal suit while meticulously preserving the subject's facial features, expression, pose, and body proportions.

## Features
-   **AI-Powered Clothing Transformation**: Utilizes the `gemini-2.0-flash-exp-image-generation` model for advanced image editing.
-   **Specific Style Application**: Transforms clothing into a realistic black formal suit.
-   **Subject Integrity Preservation**: Ensures the original face, pose, body, facial features, expression, hairstyle, and proportions remain unchanged.
-   **API Key Management**: Securely loads the Google API key from an environment variable (`.env` file).
-   **Image Upload and Download**: Handles the upload of input images to the Gemini API and saves the generated output image locally.

## Installation

To set up and run this project, follow these steps:

1.  **Clone the Repository (if applicable)**:
    If this project is part of a larger repository, clone it first:
    ```bash
    git clone https://github.com/galang006/gemini_image_styler.git
    cd gemini_image_styler
    ```

2.  **Install Dependencies**:
    Install the required Python libraries using pip:
    ```bash
    pip install google-generativeai python-dotenv
    ```

3.  **Set up Google Gemini API Key**:
    *   Obtain a Google API Key by enabling the Gemini API in your Google Cloud Project. Visit the [Google AI Studio](https://aistudio.google.com/app/apikey) or Google Cloud Console to generate one.
    *   Create a file named `.env` in the root directory of your project.
    *   Add your API key to this file in the following format:
        ```
        GOOGLE_API_KEY="YOUR_API_KEY_HERE"
        ```
        Replace `"YOUR_API_KEY_HERE"` with your actual API key.

4.  **Prepare Input Image Directory**:
    *   Create a directory named `image` in the root of your project:
        ```bash
        mkdir image
        ```
    *   Place the `.jpg` image you wish to style into this `image` directory. For example, if your image is named `DSCF0252.jpg`, place it inside the `image/` folder.

## Usage

Once the installation and setup are complete, you can run the image styler:

1.  **Ensure Input Image is Ready**:
    Make sure your `.jpg` input image is located in the `image/` directory. The `main.py` script currently expects an image named `DSCF0252.jpg`. If your image has a different name, you will need to modify the `main.py` file accordingly.

    For example, if your image is `my_photo.jpg`, change the last line of `main.py` from:
    ```python
    if __name__ == "__main__":
        generate("DSCF0252")
    ```
    to:
    ```python
    if __name__ == "__main__":
        generate("my_photo") # Use the filename without the .jpg extension
    ```

2.  **Run the Script**:
    Execute the `main.py` script from your terminal:
    ```bash
    python main.py
    ```

3.  **View Output**:
    The script will process the image and save the styled output image as `[your_image_name]_output.jpg` (e.g., `DSCF0252_output.jpg`) in the same `image/` directory. A message indicating the saved file will be printed to the console.

## Code Structure

The project has a straightforward structure:

```
.
├── .gitignore
├── main.py
└── image/
```

-   **`.gitignore`**:
    Specifies intentionally untracked files that Git should ignore, such as the `.env` file (containing your API key) and the `image/` directory (which holds generated outputs and potentially sensitive inputs).

-   **`main.py`**:
    This is the core Python script that orchestrates the entire image styling process.
    -   It loads environment variables using `dotenv`.
    -   Defines `save_binary_file(file_name, data)`: A utility function to write binary data (like images) to a file.
    -   Contains `generate(img_name)`: The main function responsible for:
        -   Initializing the `genai.Client` with the Google API key.
        -   Uploading the specified input image (`image/{img_name}.jpg`) to the Gemini API.
        -   Setting the `gemini-2.0-flash-exp-image-generation` model.
        -   Constructing a detailed prompt to instruct the model to change only the clothing to a black formal suit while preserving other subject attributes.
        -   Configuring generation parameters (`temperature`, `top_p`, `top_k`, `max_output_tokens`, `response_modalities`, `safety_settings`).
        -   Streaming the content from the Gemini model and saving the generated output image (`image/{img_name}_output.jpg`) to the local filesystem.
    -   The `if __name__ == "__main__":` block demonstrates how to call the `generate` function with a hardcoded image name, `DSCF0252`.

-   **`image/`**:
    This directory is intended to store your input images and will also be where the generated output images are saved.
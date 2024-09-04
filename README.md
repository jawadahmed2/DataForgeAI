# DataForgeAI

> [!IMPORTANT]
>
>DataForgeAI is a cutting-edge platform that transforms unstructured data into structured, actionable insights optimized for Generative AI (LLM) applications. Whether dealing with documents, tables, images, videos, audio files, or web pages, DataForgeAI ensures your data is clean, organized, and ready for advanced AI tasks such as Retrieval-Augmented Generation (RAG) and fine-tuning. Leveraging multimodal capabilities, it seamlessly integrates text, tables, and images to extract and synthesize information, making it an ideal solution for complex data environments where traditional RAG systems fall short. With powerful tools like multi-vector retrieval and the latest in LLM technology, DataForgeAI enables comprehensive data processing and intelligent answer generation from diverse content types.

## Installation

> [!IMPORTANT]
> The server only works on Linux-based systems. This is due to certain dependencies and system-specific configurations that are not compatible with Windows or macOS.

```bash
git clone https://github.com/jawadahmed2/DataForgeAI.git
cd DataForgeAI
```

Create a Virtual Environment or conda environment:

```bash
conda create -n dataforegeai-venv python=3.10
conda activate dataforgeai-venv
```

Install Dependencies:

```bash
poetry install
# or
pip install -e .
# or
pip install -r pyproject.toml
```

<!-- ### 🛳️ Docker

To use dataForgeAI with Docker, execute the following commands:

```bash
# if you are running on a gpu
docker run --gpus all -p 8000:8000 savatar101/omniparse:0.1
# else
docker run -p 8000:8000 savatar101/omniparse:0.1
```

Alternatively, if you prefer to build the Docker image locally:
Then, run the Docker container as follows:

```bash
docker build -t omniparse .
# if you are running on a gpu
docker run --gpus all -p 8000:8000 omniparse
# else
docker run -p 8000:8000 omniparse

``` -->

## Usage

Run the Server:

```bash
python server.py --host 0.0.0.0 --port 8000 --documents --media --web
```

- `--documents`: Load in all the models that help you parse and ingest documents (Surya OCR series of models and Florence-2).
- `--media`: Load in Whisper model to transcribe audio and video files.
- `--web`: Set up selenium crawler.

Download Models:
If you want to download the models before starting the server

```bash
python download.py --documents --media --web
```

- `--documents`: Load in all the models that help you parse and ingest documents (Surya OCR series of models and Florence-2).
- `--media`: Load in Whisper model to transcribe audio and video files.
- `--web`: Set up selenium crawler.

## Supported Data Types

| Type      | Supported Extensions                                |
|-----------|-----------------------------------------------------|
| Documents | .doc, .docx, .pdf, .ppt, .pptx                      |
| Images    | .png, .jpg, .jpeg, .tiff, .bmp, .heic               |
| Video     | .mp4, .mkv, .avi, .mov                              |
| Audio     | .mp3, .wav, .aac                                    |
| Web       | dynamic webpages, http://<anything>.com             |

<details>
<summary><h2>API Endpoints</h></summary>

> Client library compatible with Langchain, llamaindex, and haystack integrations coming soon.

- [API Endpoints](#api-endpoints)
  - [Document Parsing](#document-parsing)
    - [Parse Any Document](#parse-any-document)
    - [Parse PDF](#parse-pdf)
    - [Parse PowerPoint](#parse-powerpoint)
    - [Parse Word Document](#parse-word-document)
  - [Media Parsing](#media-parsing)
    - [Parse Any Media](#parse-any-media)
    - [Parse Image](#parse-image)
    - [Process Image](#process-image)
    - [Parse Video](#parse-video)
    - [Parse Audio](#parse-audio)
  - [Website Parsing](#website-parsing)
    - [Parse Website](#parse-website)

### Document Parsing

#### Parse Any Document

Endpoint: `/parse_document`
Method: POST

Parses PDF, PowerPoint, or Word documents.

Curl command:

```bash
curl -X POST -F "file=@/path/to/document" http://localhost:8000/parse_document
```

#### Parse PDF

Endpoint: `/parse_document/pdf`
Method: POST

Parses PDF documents.

Curl command:

```bash
curl -X POST -F "file=@/path/to/document.pdf" http://localhost:8000/parse_document/pdf
```

#### Parse PowerPoint

Endpoint: `/parse_document/ppt`
Method: POST

Parses PowerPoint presentations.

Curl command:

```bash
curl -X POST -F "file=@/path/to/presentation.ppt" http://localhost:8000/parse_document/ppt
```

#### Parse Word Document

Endpoint: `/parse_document/docs`
Method: POST

Parses Word documents.

Curl command:

```
curl -X POST -F "file=@/path/to/document.docx" http://localhost:8000/parse_document/docs
```

### Media Parsing

<!-- #### Parse Any Media

Endpoint: `/parse_media`
Method: POST

Parses images, videos, or audio files.

Curl command:
```
curl -X POST -F "file=@/path/to/media_file" http://localhost:8000/parse_media
``` -->

#### Parse Image

Endpoint: `/parse_image/image`
Method: POST

Parses image files (PNG, JPEG, JPG, TIFF, WEBP).

Curl command:

```
curl -X POST -F "file=@/path/to/image.jpg" http://localhost:8000/parse_media/image
```

#### Process Image

Endpoint: `/parse_image/process_image`
Method: POST

Processes an image with a specific task.

Possible task inputs:
`OCR | OCR with Region | Caption | Detailed Caption | More Detailed Caption | Object Detection | Dense Region Caption | Region Proposal`

Curl command:

```
curl -X POST -F "image=@/path/to/image.jpg" -F "task=Caption" -F "prompt=Optional prompt" http://localhost:8000/parse_media/process_image
```

Arguments:

- `image`: The image file
- `task`: The processing task (e.g., Caption, Object Detection)
- `prompt`: Optional prompt for certain tasks

#### Parse Video

Endpoint: `/parse_media/video`
Method: POST

Parses video files (MP4, AVI, MOV, MKV).

Curl command:

```
curl -X POST -F "file=@/path/to/video.mp4" http://localhost:8000/parse_media/video
```

#### Parse Audio

Endpoint: `/parse_media/audio`
Method: POST

Parses audio files (MP3, WAV, FLAC).

Curl command:

```
curl -X POST -F "file=@/path/to/audio.mp3" http://localhost:8000/parse_media/audio
```

### Website Parsing

#### Parse Website

Endpoint: `/parse_website/parse`
Method: POST

Parses a website given its URL.

Curl command:

```
curl -X POST -H "Content-Type: application/json" -d '{"url": "https://example.com"}' http://localhost:8000/parse_website
```

Arguments:

- `url`: The URL of the website to parse

</details>

## Limitations

There is a need for a GPU with 8~10 GB minimum VRAM as we are using deep learning models.
\

Document Parsing Limitations
\

- [Marker](https://github.com/VikParuchuri/marker) which is the underlying PDF parser will not convert 100% of equations to LaTeX because it has to detect and then convert them.
- It is good at parsing english but might struggle for languages such as Chinese
- Tables are not always formatted 100% correctly; text can be in the wrong column.
- Whitespace and indentations are not always respected.
- Not all lines/spans will be joined properly.
- This works best on digital PDFs that won't require a lot of OCR. It's optimized for speed, and limited OCR is used to fix errors.
- To fit all the models in the GPU, we are using the smallest variants, which might not offer the best-in-class performance.

## Acknowledgements

This project builds upon the remarkable [Marker](https://github.com/VikParuchuri/marker) project created by [Jawad Ahmed]. I express my gratitude for the inspiration and foundation provided by this project. Special thanks to all the members for their contributions in this project.

Models being used:

- OCR, Detect, Layout, Order, and Texify
- Florence-2 base
- Whisper Small

---

## Contact

For any inquiries, please contact me at [Email](jawad.kohat2002@gmail.com)

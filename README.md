# scene-otsu

A Python library for scene splitting SRT subtitle files using the Otsu method. Uses OpenAI's embedding models to find semantically appropriate scene boundaries.

## Features

- **SRT Subtitle Parsing**: Parse SRT format subtitle files
- **Otsu Method Scene Splitting**: Recursively apply multi-dimensional Otsu method to detect semantically appropriate scene boundaries
- **OpenAI Embeddings**: Use OpenAI's embedding models to calculate semantic similarity of text
- **Token Limit Support**: Split scenes based on specified maximum token count

## Installation

```bash
pip install scene-otsu
```

## Usage

### Basic Example

```python
from scene_otsu import SceneSplitter

# Set OpenAI API key
api_key = "your-openai-api-key"

# Initialize SceneSplitter
splitter = SceneSplitter(api_key=api_key)

# Process SRT string
srt_content = """
1
00:00:00,000 --> 00:00:05,000
Hello, welcome to this video.

2
00:00:05,000 --> 00:00:10,000
Today we will discuss machine learning.

3
00:00:10,000 --> 00:00:15,000
Let's start with the basics.
"""

# Execute scene splitting (max 200 tokens)
result = splitter.process(srt_content, max_tokens=200)
print(result)
```

### Using SubtitleParser

```python
from scene_otsu import SubtitleParser

# Parse SRT string
srt_content = """
1
00:00:00,000 --> 00:00:05,000
First subtitle

2
00:00:05,000 --> 00:00:10,000
Second subtitle
"""

# Basic parsing
subtitles = SubtitleParser.parse_srt_string(srt_content)
# Returns: [("00:00:00,000", "00:00:05,000", "First subtitle"), ...]

# Parse as scene information
scenes = SubtitleParser.parse_srt_scenes(srt_content)
# Returns: [{"index": 1, "start_time": "00:00:00,000", "end_time": "00:00:05,000", ...}, ...]
```

## API Reference

### SceneSplitter

#### `__init__(api_key: str, model: str = "text-embedding-3-small", batch_size: int = 16)`

Initialize SceneSplitter.

**Parameters:**
- `api_key`: OpenAI API key
- `model`: OpenAI embedding model to use (default: "text-embedding-3-small")
- `batch_size`: Batch size for embedding generation (default: 16)

#### `process(srt_string: str, max_tokens: int = 200) -> str`

Process SRT string and return scene-split SRT string.

**Parameters:**
- `srt_string`: Input SRT string
- `max_tokens`: Maximum tokens per chunk (default: 200)

**Returns:**
- Scene-split SRT string

### SubtitleParser

#### `parse_srt_string(srt_string: str) -> List[Tuple[str, str, str]]`

Parse SRT string.

**Returns:**
- List in the format `[(start_timestamp, end_timestamp, text), ...]`

#### `parse_srt_scenes(srt_string: str) -> List[Dict[str, Any]]`

Convert SRT to scene-based dictionary.

**Returns:**
- List in the format `[{index, start_time, end_time, start_sec, end_sec, text}]`

#### `parse_timestamp(timestamp: str) -> float`

Convert timestamp string to seconds.

**Parameters:**
- `timestamp`: Timestamp string (e.g., "00:00:05,000")

**Returns:**
- Seconds (float, including milliseconds)

## Requirements

- Python 3.11 or higher
- OpenAI API key

## Dependencies

- numpy >= 2.3.5
- openai >= 2.8.1
- scikit-learn >= 1.7.2
- tiktoken >= 0.12.0
- tqdm >= 4.67.1

## License

MIT License

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Author

Yuki Harada

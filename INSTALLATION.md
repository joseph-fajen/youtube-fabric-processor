# YouTube Fabric Processor - Installation Guide

## Overview

This application processes YouTube videos through fabric patterns to extract insights, summaries, and knowledge. The **OPTIMIZED VERSION** processes 13 patterns in ~70 seconds using transcript-first architecture with parallel processing.

## Prerequisites

- **Node.js** (version 14 or higher)
- **Go** (for Fabric CLI installation)
- **Python** (for yt-dlp)
- **Git**

## Quick Setup

### 1. Clone and Install Dependencies

```bash
git clone https://github.com/joseph-fajen/youtube-fabric-processor.git
cd youtube-fabric-processor
npm install
```

### 2. Install External Dependencies

Run the automated setup:
```bash
npm run setup
```

Or install manually:

#### Install Fabric CLI (Required)
```bash
# Option 1: Go install (recommended)
go install github.com/danielmiessler/fabric@latest

# Option 2: Homebrew (macOS only)
brew install fabric

# Option 3: Manual download from GitHub releases
# Visit: https://github.com/danielmiessler/fabric/releases
```

#### Install yt-dlp (For YouTube Metadata)
```bash
# Python/pip
pip install yt-dlp

# Or with pipx
pipx install yt-dlp

# Or homebrew (macOS)
brew install yt-dlp
```

### 3. Configure Fabric

Set up Fabric with your AI provider:
```bash
fabric --setup
```

Follow the prompts to configure:
- Choose your AI provider (OpenAI, Anthropic, etc.)
- Enter API keys
- Set default model (recommend `gpt-4o-mini` for speed)

### 4. Start the Application

```bash
npm start
```

The application will automatically find an available port (starting from 3000) and display the URL. No conflicts with other local servers!

## Performance Modes

The application automatically selects the best processing method:

### 1. **Transcript-First Mode** (~70 seconds)
- Downloads YouTube transcript once, processes through all patterns
- Eliminates 13 separate YouTube downloads
- Processes 3 patterns simultaneously with local transcript
- **Requires**: yt-dlp + Fabric CLI

### 2. **Legacy CLI Mode** (5-15 minutes)
- Uses Fabric CLI with sequential processing
- Fallback when transcript download fails
- **Requires**: Fabric CLI only

### 3. **Simulation Mode** (~30 seconds)
- Mock processing for testing
- Used when no fabric installation detected
- **Requires**: Nothing (built-in)

## Features Overview

### Web Interface
- **Simple URL Input**: Enter any YouTube video URL
- **Real-time Progress**: Watch as 13 fabric patterns execute
- **Professional Output**: Download text files with markdown formatting
- **Error Handling**: Graceful fallback and user feedback

### Processing Pipeline
1. **Phase 1**: Primary extraction (summary, core message)
2. **Phase 2**: Content analysis (wisdom, insights, ideas, patterns, recommendations, predictions)
3. **Phase 3**: Knowledge graph building (references, questions, tags)
4. **Phase 4**: Synthesis materials (summary, flashcards)

### Output Files
- 13 numbered text files (`.txt` extension)
- Professional markdown formatting
- ZIP download for easy sharing
- Enhanced index file with video metadata
- **Descriptive folder names**: `2025-07-15_Video-Title_a1b2c3d4/`
- **Descriptive downloads**: `Video-Title_analysis_a1b2c3d4.zip`

## Video Length Recommendations

**IMPORTANT**: This application is optimized for shorter videos. Fabric patterns are designed for complete content analysis and lose significant effectiveness when content is chunked.

### Recommended Limits:
- **✅ Optimal (up to 2 hours)**: Full effectiveness with all 13 fabric patterns
- **⚠️ Acceptable (2-3 hours)**: Good results, may hit model token limits  
- **❌ Not Recommended (3+ hours)**: Significantly reduced pattern effectiveness (~25% quality)

### Technical Reasoning:
- Fabric patterns require complete context for effective analysis
- Chunking breaks narrative flow and cross-references
- Token limits: ~50K per model, ~250 tokens/minute average speech
- Beyond 3 hours requires chunking which fundamentally compromises fabric pattern design

## Usage Examples

### Example 1: Simon Willison Video
```
URL: https://youtu.be/YpY83-kA7Bo?si=CbPFLzMVkm1h1z8f
Expected: 13 analysis files covering AI developments
Processing time: ~70 seconds (transcript-first) or ~30 seconds (simulation)
```

### Example 2: Educational Content
```
URL: https://www.youtube.com/watch?v=YOUR_VIDEO_ID
Expected: Comprehensive educational analysis
Output: Professional text files ready for further use
```

## System Modes

### Full Processing Mode
- **Requirements**: Fabric CLI installed and configured
- **Features**: Complete fabric pattern execution
- **Output**: Actual AI-generated analysis
- **Status**: "Fabric integration ready"

### Simulation Mode
- **Requirements**: None (automatic fallback)
- **Features**: Simulated processing with example content
- **Output**: Sample analysis structure
- **Status**: "Fabric not available (simulation mode)"

## Troubleshooting

### Common Issues

#### "Fabric command not found"
```bash
# Check if fabric is in PATH
which fabric

# If not found, ensure Go bin is in PATH
export PATH=$PATH:$(go env GOPATH)/bin

# Or add to your shell profile (~/.bashrc, ~/.zshrc)
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.zshrc
```

#### "Pattern execution failed"
1. Verify fabric setup: `fabric --setup`
2. Test fabric directly: `fabric -p extract_wisdom "test input"`
3. Check API keys and model availability
4. Try with different model: `fabric -m gpt-3.5-turbo`

#### "YouTube metadata extraction failed"
```bash
# Test yt-dlp installation
yt-dlp --version

# Test with a video
yt-dlp --dump-json --no-download "https://youtube.com/watch?v=VIDEO_ID"
```

#### Server Issues
1. **Server won't start**
   - Check Node.js installation: `node --version`
   - Reinstall dependencies: `rm -rf node_modules && npm install`

2. **YouTube URL not accepted**
   - Ensure URL contains youtube.com or youtu.be
   - Check for complete URL format

3. **Processing fails**
   - Check fabric installation
   - Verify internet connection
   - Try simulation mode first

### Performance Optimization

#### For Maximum Speed:
1. Use `claude-3-5-sonnet-20241022` as default model (optimal speed/cost balance)
2. Ensure stable internet connection
3. Use SSD storage for outputs
4. Process videos under 2 hours for best results

#### Resource Usage:
- **RAM**: ~200MB base + ~50MB per concurrent pattern
- **CPU**: Moderate during processing
- **Network**: High during AI model calls
- **Storage**: ~1-5MB per processed video

### Testing the System
```bash
# Run built-in tests
node test.js

# Check system health
curl http://localhost:3000/health

# Test fabric patterns directly
fabric -p youtube_summary "https://youtu.be/example"
```

## Project Structure

```
youtube-fabric-processor/
├── public/                 # Web interface files
│   ├── index.html         # Main HTML interface
│   ├── styles.css         # Professional styling
│   └── app.js             # Client-side JavaScript
├── fabric-patterns.js     # Pattern definitions
├── fabric-integration.js  # Fabric MCP integration
├── server.js              # Express server
├── package.json           # Dependencies
├── start.sh              # Startup script
├── test.js               # System validation
└── README.md             # Project documentation
```

## Next Steps

1. **Start the application** with `./start.sh`
2. **Test with a sample video** to verify functionality
3. **Install Fabric CLI** for full processing capabilities
   ```bash
   go install github.com/danielmiessler/fabric@latest
   fabric --setup  # Configure API keys and models
   ```
4. **Customize patterns** by modifying `fabric-patterns.js`
5. **Extend functionality** by adding new fabric patterns

## Success Criteria

✅ **All implemented features working**
- Web interface responsive and professional
- 13 fabric patterns executing correctly
- Real-time progress tracking functional
- ZIP download working properly
- Error handling graceful
- Both simulation and full modes operational

The YouTube Fabric Processor is ready for use!
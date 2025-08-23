# RIME TTS Extension

A WebSocket-based Text-to-Speech extension for the TEN Framework using RIME TTS API.

## Features

- **Real-time streaming TTS**: Supports streaming audio generation with low latency
- **Multiple audio formats**: MP3, MULAW, PCM support
- **Advanced voice models**: Mist v2 and Mist models available
- **Flexible text processing**: Support for text segmentation, speed control, and custom pronunciation
- **Robust connection management**: Automatic reconnection and error handling
- **Comprehensive logging**: Detailed logging for debugging and monitoring

## Configuration

### Required Parameters

- `api_key`: Your RIME TTS API key (Bearer token)
- `speaker`: Voice speaker name (e.g., "cove", "nova", etc.)

### Optional Parameters

- `model_id`: TTS model to use ("mistv2" or "mist", default: "mistv2")
- `audio_format`: Audio output format ("mp3", "mulaw", "pcm", default: "mp3")
- `lang`: Language code (default: "eng")
- `sampling_rate`: Sample rate in Hz (4000-44100, default: 22050)
- `speed_alpha`: Speech speed multiplier (0.1-10.0, default: 1.0)
- `reduce_latency`: Enable latency reduction (default: false)
- `segment`: Text segmentation mode ("immediate", "never", "bySentence", default: "bySentence")
- `pause_between_brackets`: Add pauses between angle brackets (default: false)
- `phonemize_between_brackets`: Enable phonemization between curly brackets (default: false)
- `inline_speed_alpha`: Inline speed control values (comma-separated)

### Example Configuration

```json
{
    "dump": false,
    "dump_path": "./",
    "params": {
        "api_key": "${env:RIME_TTS_API_KEY}",
        "speaker": "cove",
        "model_id": "mistv2",
        "audio_format": "mp3",
        "lang": "eng",
        "sampling_rate": 22050,
        "speed_alpha": 1.0,
        "reduce_latency": false,
        "segment": "bySentence"
    }
}
```

## Environment Variables

Set the following environment variable:

```bash
export RIME_TTS_API_KEY="your_rime_api_key_here"
```

## Usage

### Basic Usage

The extension automatically handles:
- WebSocket connection establishment
- Text streaming and audio reception
- Connection reconnection on failures
- Audio data processing and delivery

### Supported Operations

- **Text synthesis**: Send text for TTS conversion
- **Clear buffer**: Clear accumulated text buffer
- **Flush**: Force synthesis of remaining buffer
- **End of stream (EOS)**: Complete synthesis and close connection

### Message Types

The extension handles these RIME TTS message types:
- `chunk`: Audio data chunks (base64 encoded)
- `timestamps`: Word-level timing information
- `error`: Error messages from the server

## Testing

Run the test script to verify your setup:

```bash
python test_rime_tts.py
```

This will:
1. Connect to RIME TTS with the "cove" speaker
2. Send test messages
3. Receive and save audio output
4. Display timing information

## Architecture

### Core Components

1. **RimeTTSynthesizer**: Main synthesizer class handling WebSocket communication
2. **RimeTTSClient**: Client wrapper managing synthesizer lifecycle
3. **RimeTTSExtension**: TEN Framework extension interface

### Key Features

- **Asynchronous design**: Full async/await support for high performance
- **Queue-based messaging**: Reliable message delivery between components
- **Event-driven architecture**: Clean separation of concerns
- **Error recovery**: Automatic reconnection and error handling
- **Resource management**: Proper cleanup of connections and resources

## Error Handling

The extension handles various error scenarios:
- Network connection failures
- Authentication errors
- Invalid message formats
- Server-side errors
- Timeout conditions

## Performance Considerations

- **Connection pooling**: Efficient connection management
- **Streaming audio**: Real-time audio delivery
- **Memory management**: Proper cleanup of audio buffers
- **Latency optimization**: Configurable latency reduction options

## Troubleshooting

### Common Issues

1. **Authentication failed**: Check your API key
2. **Connection timeout**: Verify network connectivity
3. **Invalid speaker**: Ensure speaker name is correct
4. **Audio format issues**: Check supported audio formats

### Debug Mode

Enable debug logging by setting the log level to DEBUG in your TEN Framework configuration.

## API Reference

For detailed API documentation, visit: [RIME TTS API Documentation](https://docs.rime.ai/api-reference/endpoint/websockets-json)

## License

This extension is part of the TEN Framework and is licensed under the Apache License, Version 2.0.

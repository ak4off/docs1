# API Reference

Details on API request and response protocols for the ASR, MT, TTS and Video Interpolation services have been provided here.

## ASR (Speech-to-Text)

The endpoint for ASR service is [https://asr.iitm.ac.in/internal/asr/decode](https://asr.iitm.ac.in/internal/asr/decode)

### Request keys

| Key               | Description                                                                         |
| :---              | :----                                                                               |
| `file`            | the media file to be transcribed                                                    |
| `language`        | the language of the source audio/video in all lowercase (eg: `hindi`, `english`)    |
| `vtt` (optional)  | whether a webVTT caption file has to be generated. This is an optional value. It accepts two string values either `true` or `false`. By default, this is `false`. This can be used for captioning purposes.|


### Response keys

Upon successful service of the request, the API returns a JSON response with the following keys:

| Key               | Description                                                                         |
| :---              | :----                                                                               |
| `status`          | success                                                                             |
| `time_taken`      | time taken to transcribe the given audio/video in seconds                           |
| `transcript`      | the transcription of the given speech input, as infered by the deployed model       |
| `vtt`             | WebVTT caption if it was requested. i.e, if the vtt key was set to `true` in the request, a WebVTT caption would be returned.|

In case of a service failure, the API returns a JSON response with the following keys:

| Key               | Description                                                                         |
| :---              | :----                                                                               |
| `status`          | failure                                                                             |
| `reason`          | a reason for the failure in serving the request                                     |


### Supported Languages

- [x] Bengali
- [x] English
- [x] Gujarati
- [x] Hindi
- [x] Kannada
- [x] Malayalam
- [x] Marathi
- [x] Odia
- [x] Punjabi
- [x] Sanskrit
- [x] Tamil
- [x] Telugu
- [x] Urdu

### Usage

=== "Python"

    ``` py
    import requests

    files = {
    'file': open('speech_sample1.mp3', 'rb'),
    'language': (None, 'hindi'),
    'vtt': (None, 'true'),
    }

    response = requests.post('https://asr.iitm.ac.in/internal/asr/decode', files=files)
    print(response.json())
    ```

=== "cURL"

    ``` sh
    curl -v -X POST -F 'file=@speech_sample2.wav' -F 'language=english' \
    -F 'vtt=true' https://asr.iitm.ac.in/internal/asr/decode
    ```

Sample audio files to test the API: [english speech](https://drive.google.com/file/d/1ucJCfKfKr00_09H8_FmW57xFYHYPzC2h/view?usp=share_link), [tamil speech](https://drive.google.com/file/d/19tgIL2YeZ-vU9eABePeBL_O1f1jWp2Sv/view?usp=share_link).

The ASR API accepts media files from most of the common formats such as `.mp3`, `.mp4`, `.wav`, `.ogg` etc.

Web Demo interface available at [https://asr.iitm.ac.in/asr/v2](https://asr.iitm.ac.in/asr/v2)
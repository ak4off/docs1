# MT (Text-to-Text)

The endpoint for the Machine Translation service is [https://asr.iitm.ac.in/internal/asr/decode](https://asr.iitm.ac.in/internal/asr/decode).
This API supports the translation services translation services provided by research groups at IITB, IIIT-H and Meta AI. While we internally host the [NLLB model](https://ai.facebook.com/research/no-language-left-behind/) from Meta AI, the translations from IITB / IIIT-H are obtained through internal API calls to their systems.

### Request keys

| Key                       |Description                                                                |
| :---                      | :----                                                                     |
| `src_language`            | source language of the text to be translated in lowercase (eg: `hindi`, `telugu`). Default option: `english`.|
| `tgt_language`            | target language to which the input data is to be translated into. all in lowercase (eg: `sanskrit`, `urdu`). Default option: `english`.|
| `transcript` (optional)   | source text being passed for translation.                                 |
| `source_vtt` (optional)   | data being passed for translation as a string, but with captions as in a `.vtt` file. Each segment/caption of the vtt is translated independently.                                 |
| `translator_choice`       | the translator option being chosen to translate the input. Choose from `iiit-h`, `iitb`, `meta_ai`. Default option: `meta_ai`.|


### Response keys

Upon successful service of the request, the API returns a JSON response with the following keys:

| Key               | Description                                                                         |
| :---              | :----                                                                               |
| `status`          | success                                                                             |
| `mt_out`          | If the `transcript` key is passed with some text in the request, `mt_out` would carry the translation of the same. |
| `translated_vtt`  | If the `source_vtt` key is passed with vtt content in the request, `translated_vtt` would carry the translation of each of the captions in the `source_vtt` and returns the translated vtt content.|
| `transcript`      | If the `transcript` key is passed with some text in the request, additional punctuations would have been incorporated into the text and the richer text is returned. |
| `source_vtt`      | If the `source_vtt` key is passed with vtt content in the request, additional punctuations would have been added to each of the captions in the `source_vtt` and the richer source vtt content is returned. |

In case of a service failure, the API returns a JSON response with the following keys:

| Key               | Description                                                                         |
| :---              | :----                                                                               |
| `status`          | failure                                                                             |
| `reason`          | a reason for the failure in serving the request                                     |


### Language pairs supported by `iiit-h`

- [x] English --> Gujarati
- [x] English --> Hindi
- [x] English --> Punjabi
- [x] English --> Tamil
- [x] English --> Telugu
- [x] Hindi --> Bangla
- [x] Hindi --> Gujarati
- [x] Hindi --> Punjabi
- [x] Hindi --> Tamil
- [x] Hindi --> Telugu
- [x] Marathi --> English
- [x] Marathi --> Hindi
- [x] Gujarati --> Hindi

### Language pairs supported by `iitb`

- [x] English --> Hindi
- [x] English --> Marathi
- [x] Hindi --> Marathi
- [x] Marathi --> English

### Language pairs supported by `meta_ai`

Every possible set of language pairs from the below set is supported by the `meta_ai` translator choice.
``` py
{'English', 'Hindi', 'Assamese', 'Awadhi', 'Bengali', 'Bhojpuri', 'Gujarati', 
'Chattisgarhi', 'Kannada', 'Kashmiri', 'Malayalam', 'Maithili', 'Marathi', 
'Odia', 'Punjabi', 'Sanskrit', 'Urdu', 'Telugu', 'Tamil', 'Arabic'}
```

### Usage

=== "Python"

    ``` py
    import requests

    text = "सुप्रभात. आईआईटी मद्रास में आपका स्वागत है। "

    payload = {
    'src_language': 'hindi',
    'tgt_language': 'english',
    'transcript': text,
    'source_vtt': None,
    'translator_choice': 'meta_ai'
    }

    response = requests.post('https://asr.iitm.ac.in/internal/asr/decode', data = payload).json()
    print(response)
    ```

=== "cURL"

    ``` sh
    curl -v -X POST -F 'src_language=english' -F 'tgt_language=telugu' \
    -F 'transcript=Welcome to our lab' -F 'translator_choice=iiit-h' \
    https://asr.iitm.ac.in/internal/asr/decode
    ```

Web Demo interface for the NLLB model is available at [https://asr.iitm.ac.in/demo/ttt](https://asr.iitm.ac.in/demo/ttt)
---
title: "snowflake.snowpark.functions.ai_transcribe | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark/reference/python/latest/snowpark/api/snowflake.snowpark.functions.ai_transcribe.html
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.snowpark.functions.ai_transcribe¶

snowflake.snowpark.functions.ai_transcribe(_audio_file : [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")_, _return_error_details : Optional[bool] = None_, _** kwargs_) → [Column](snowflake.snowpark.Column.html#snowflake.snowpark.Column "snowflake.snowpark.column.Column")[[source]](https://github.com/snowflakedb/snowpark-python/blob/v1.54.0/src/snowflake/snowpark/functions.py#L13721-L13856)¶
    

Transcribes text from an audio file with optional timestamps and speaker labels.

AI_TRANSCRIBE supports numerous languages (automatically detected), and audio can contain more than one language. Timestamps and speaker labels are extracted based on the specified timestamp granularity.

Parameters:
    

  * **audio_file** – A FILE type column representing an audio file. The audio file must be on a Snowflake stage that uses server-side encryption and is accessible to the user. Use the to_file() function to create a reference to your staged file.

  * **return_error_details** – When `True`, returns an OBJECT with `value` and `error` fields instead of returning NULL on failure.

  * ****kwargs** – 

Configuration settings specified as key/value pairs. Supported keys:

    * timestamp_granularity: A string specifying the desired timestamp granularity. Possible values are:

      * ’word’: The file is transcribed as a series of words, each with its own timestamp.

      * ’speaker’: The file is transcribed as a series of conversational “turns”, each with its own timestamp and speaker label.

If this field is not specified, the entire file is transcribed as a single segment without timestamps by default.



Returns:
    

A string containing a JSON representation of the transcription result. The JSON object contains the following fields:

>   * audio_duration: The total duration of the audio file in seconds.
> 
>   * text: The transcription of the complete audio file (when timestamp_granularity is not specified).
> 
>   * segments: An array of segments (when timestamp_granularity is set to ‘word’ or ‘speaker’). Each segment contains:
> 
>     * start: The start time of the segment in seconds.
> 
>     * end: The end time of the segment in seconds.
> 
>     * text: The transcription text for the segment.
> 
>     * speaker_label: The label of the speaker for the segment (only when timestamp_granularity is ‘speaker’). Labels are of the form “SPEAKER_00”, “SPEAKER_01”, etc.
> 
> 


Note

  * Supports languages: Arabic, Bulgarian, Cantonese, Catalan, Chinese, Czech, Dutch, English, French, German, Greek, Hungarian, Indonesian, Italian, Japanese, Korean, Latvian, Polish, Portuguese, Romanian, Russian, Serbian, Slovenian, Spanish, Swedish, Thai, Turkish, Ukrainian.

  * Supported audio formats: FLAC, MP3, Ogg, WAV, WebM

  * Maximum file size: 700 MB

  * Maximum duration: 60 minutes with timestamps, 120 minutes without




Examples:
[code] 
    >>> import json
    >>> # Basic transcription without timestamps
    >>> _ = session.sql("CREATE OR REPLACE TEMP STAGE mystage ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE')").collect()
    >>> _ = session.file.put("tests/resources/audio.ogg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_transcribe(to_file("@mystage/audio.ogg")).alias("transcript")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> result['audio_duration'] > 120  # more than 2 minutes
    True
    >>> "glad to see things are going well" in result['text'].lower()
    True
    
    >>> # Transcription with word-level timestamps
    >>> df = session.range(1).select(
    ...     ai_transcribe(
    ...         to_file("@mystage/audio.ogg"),
    ...         timestamp_granularity='word'
    ...     ).alias("transcript")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> len(result["segments"]) > 0
    True
    >>> result["segments"][0]["text"].lower()  
    'the'
    >>> 'start' in result["segments"][0] and 'end' in result["segments"][0]
    True
    
    >>> # Transcription with speaker diarization
    >>> _ = session.file.put("tests/resources/conversation.ogg", "@mystage", auto_compress=False)
    >>> df = session.range(1).select(
    ...     ai_transcribe(
    ...         to_file("@mystage/conversation.ogg"),
    ...         timestamp_granularity='speaker'
    ...     ).alias("transcript")
    ... )
    >>> result = json.loads(df.collect()[0][0])
    >>> result["audio_duration"] > 100  # more than 100 seconds
    True
    >>> len(result["segments"]) > 0
    True
    >>> result["segments"][0]["speaker_label"]  
    'SPEAKER_00'
    >>> 'jenny' in result["segments"][0]["text"].lower()  
    True
    >>> 'start' in result["segments"][0] and 'end' in result["segments"][0]  
    True
    
[/code]

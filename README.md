# Top level Structure
Each container has, 5 attributes: 5 dictionaries.
Each dictionary holds a specific type of information:

![Overview](github-documentation/corpus-structure-top-level.svg)




## speech_data

self.speech_data`['key']` | content
--- | ---
['samples'] | number of samples in the recording
['sample_rate'] | sample rate of recording
['length'] | length in seconds of the recording
['orig_lang'] | (not yet set)
['orig_lang_kaldi'] | original language as detected by kaldi
['kaldi_data'] | data for kaldi decision
['orig_lang_zcr'] | original language as determined by zcr-comparison
['zcr_data'] | data for zcr decision
['orig_lang_whisper'] | original language as determined by whisper
['whisper_data'] | data for whisper decision
['orig_lang_sub'] | original language determined by subtracting channels
['sub_data'] | data for subtraction decision
['diarization'] | speaker diarization from pyannote
['window'] | most likely speaking window
['date'] | date of the session (sometimes =/= date of the speech)
['time'] | time of the speech
['session'] | number of the session (1-96)
['location'] | location of the session (Strasbourg or Brussels)
['cycle'] | number in cycle (4 in Strasbourg, 2 in Brussels)
['subject'] | topic the speech belongs to
['chair'] | person presiding during the speech
['orig_w_verbatim_langs'] | original language according to europarl-scrape matching
['orig_lang_ed'] | original language according to europarl-direct matching
['orig_lang_eu'] | original language according to europarl matching




## speaker_data

self.speaker_data`['key']` | content
--- | ---
['name'] | name of the speaker according to europarl-scrape match
['group'] | political group of the speaker accroding to europarl-scrapt
['gender'] | gender of the speaker
['birthday'] | birthday of the speaker
['age'] | age of the speaker at the time of the speech
['native'] | native language of the speaker
['wiki'] | wiki link to the speaker
['ed_name'] | name of the speaker according to europarl-direct
['ed_aff'] | political group of the speaker according to europarl-direct
['eu_name'] | name of the speaker according to europarl
['eu_aff'] | political group of the speaker according to europarl-direct
                                            
                      
                      
                      
## language_data

language_data['eng']`['key']` | content
--- | ---
['df_transcription'] | Kaldi transcription, phrase level time aligned 
['df_confidence'] | Kaldi confidence, phrase level
['df_is_translation'] | Kaldi translation, phrase level
['df_manually_corrected'] | Whether checked by a human or not, phrase level
['relay_interp'] | (not yet set)
['retour_interp'] | (not yet set)
['is_translation_perc'] | Kaldi decision if translation or not
['speech_duration'] | Speech duration (sample rate * samples)
['verbatim_file'] | europarl-direct verbatim file based on Kaldi
['verbatim_speech'] | europarl-direct verbatim report based on Kaldi
['verbatim_ratio'] | europarl-direct sequence matcher ratio based on Kaldi
['whisper'] | whisper transcription
['interpreter_window'] | most likely (interpreter) speaking window
['w_verbatim_speech'] | europarl-scrape verbatim speech based on Whisper
['w_verbatim_ratio'] | europarl-scrape sequence matcher ratio based on Whisper
['w_verbatim_file'] | europarl-scrape verbatim file based on Whisper
['p_verbatim_speech'] | Parallelised text according to Whisper + europarl-direct
['ed_file'] | europarl-direct verbatim file based on Whisper
['ed_ratio'] | europarl-direct sequence matcher ratio based on Whisper
['ed_speech'] | europarl-direct verbatim speech based on Whisper
['eu_file'] | europarl verbatim file based on Whisper
['eu_ratio'] | europarl sequence matcher ratio based on Whisper
['eu_speech'] | europarl verbatim speech based on Whisper




## file_data

File Data holds information about the mp3, eaf, folder and file. This will only work if the whole corpus tree is cloned from GitHub or the corpus structure is reassembled by hand.

self.file_data`['key']` | content
--- | ---
['mp3_paths'] | path to the original mp3 
['eaf_path'] | path to the eaf
['folder_name'] | folder name for the recording
['file_name'] | current filename
                    



## _eaf_data¶

The eaf data is not supposed to be accessed, this just holds information to (re-)build an eaf

self._eaf_data`['key']` | content
--- | ---
['time_dictionary'] | the time dictionary data from the original eaf
['eaf_duration'] | the last entry in the time dictionary
['date'] | the date from the eaf name
['property_tag'] | the property tag contents from the eaf




# Detailed Overview

## speech_data details
***
**['samples']**

The number of samples that have been extracted from the \_1\_und mp3 using [librosa](https://librosa.org/doc/latest/index.html)
***
**['sample_rate']**
 
The sample rate that has been extracted from the \_1\_und mp3 using [librosa](https://librosa.org/doc/latest/index.html)
***
**['length']**
 
The length, calculated from sample_rate * samples
***
**['orig_lang']**
 
(unset)
***
**['orig_lang_kaldi']**
 
The original language detected by Kaldi. This is documented in a function within container_utils.
The function weights each language detected by Kaldi according to its segment duration(s).
***
**['kaldi_data']**
 
The most highly weighted languages for comparison purposes.
***
**orig_lang_zcr']**
 
The original language detected by the zcr method. This is documented in a function within container_utils.
This function calculates the zero-crossing-rate in windows for each audio channel.
Interpreters often speak "on top" of a muffled original speaker channel, therefore the audio signal has a systematically different structure than the audio signal that contains only one person speaking. Their microphones may also be different and they are often too close to the microphone. Last but not least the underlying noise level is a lot lower in the cabin.
This is not always reliable because sometimes there is no interpreter and sometimes the interpreter is tech-savvy enough to completely remove the original speaker channel.
***
**['zcr_data']**
 
The zcr data for comparison purposes.
***
**['orig_lang_whisper']**

The original language detected by Whisper. This is done by providing whisper with 15 second windows (+15 seconds of silence) in 3 second leaps over the whole recording.
The result is a list of tuples containing the language and the number of windows assigned to this language (created via collections.Counter)
Please mind some idiosyncrasies, above all Whisper likes to classify British English as 'cy' (Welsh) and also tends to favor the larger languages within a family.
***
**['whisper_data']**

The full list of tuples including the language and the number of windows assigned to this language.
Please mind some idiosyncrasies, above all Whisper likes to classify British English as 'cy' (Welsh) and also tends to favor the larger languages within a family.
***
**['orig_lang_sub']**

The original language detected by the subtraction method. This is documented in a function within container_utils.
This is a simple method that inverts the waveform of the original recording and subtracts it from the waveform of the audio channels. The idea is that if there is no interpreter then the waveforms should be identical and cancel each other out.
This is not always reliable, because sometimes there is no interpreter for a language that is not the original and sometimes the channel deviates from the original channel for an unknown reason.
***
**['sub_data']**

The sub data for comparison purposes.
***
**['diarization']**

This contains a pandas dataframe with the information how many speakers there are and which speaker speaks when. This was done with [pyannote](https://github.com/pyannote/pyannote-audio) using English settings, so language changes are often seen as language changes. There is as of now no language independent diarization library, although there are theoretical papers on how to build one. It is recommended to rerun this for more precise measurements if a pretrained model for the target language is available.
***
**['window']**

--I am not sure right now if I ever put this in the container--
This contains the recording window of the speaker who speaks the longest in the recording.

***
**['date']**

The date of the _session_. this may be different from the time of the speech, because some sessions go past midnight.
***
**['time']**

The time of the recording according to the filename provided by the [Eurpean Parliament](https://multimedia.europarl.europa.eu/en/webstreaming)
***
**['session']**

The number of the session (starts with 1 on 01.09.2008 and ends with 96 in 2010)
***
**['location']**

The location of the session: Strasbourg or Brussels. This may be relevant for phonetics or accuracy of the subtraction method, because of technical differences and different phonetics (Brussels is wood, Strasbourg is glass + cloth)
***
**['cycle']**

The number within the cycle (Brussels has a cycle of 2, Strasbourg has a cycle of 4 sessions)
***
**['subject']**

The agenda item under which the speech was held. This is quite reliable and very useful for excluding "Voting Sessions" and "Question Time Sessions", which follow their own rules.
***
**['chair']**

The person sitting in the president's chair during the speech. This may be used for more precise cuts of the speech, since the president's speaker embedding is usually known.
***
**['orig_w_verbatim_langs']**

The original language as provided by a matched europarl-scrape report. Very reliable if sequence matching ratio > 0.4.
***
**['orig_lang_ed']**

The original language as provided by matched europarl-direct report. The most reliable, but most reliable source (it's vital to how europarl-direct works), but also the most scarce. Very reliable if sequence matching ratio > 0.4.
***
**['orig_lang_eu']**

The original language as provided by matched europarl report. This may be the least reliable, because this did not fill in the source language (English reports do not specify English, German reports do not specify German), so the best match often does not provide the language.
***
## speaker_data details
Speaker data has been obtained by first scraping the list of MEPs for the period 2004-2009 and 2009-2014 from Wikipedia. The English and German Wikipedia page were used for this step. This list often already contained the link to the individual Wikipedia page for each MEP. But if it didn't, an automatic search was performed. If this still yielded nothing (or a disambiguation page - I did not make an automatic decision there), the Wikipedia entry in the MEPs country's (as provided in the Wikipedia list) main language was used. This has all been performed automatically, but no errors were found in a sample of each step.

On top of that the European Parliament provides (semantically tagged!) MEP information pages. These pages unfortunately get deleted after a while, so these were not available for all members from 20 years ago. This data was considered more reliable and generally replaced the Wikipedia info. There was no gender information on the MEPs' pages, so gender comes exclusively from Wikipedia pronoun counting.

The speaker data has been linked to each speech by using the Python sequence matcher to match the transcription of the speech and its verbatim report, which contains the name and political affiliation of the speaker (see language_data for more information).
***
**['name']**

The name of the speaker. Provided by europarl, overwritten if europarl-scrape differs, overwritten again if europarl-direct differs - in theory. They don't actually differ and if they ever do I'll hunt the error down in great detail.
***
**['group']**

Political group of the speaker. Provided by europarl, overwritten if europarl-scrape differs, overwritten again if europarl-direct differs - in theory. They don't actually differ and if they ever do I'll hunt the error down in great detail.
***
**['gender']**

Gender of the speaker. This has been automatically obtained by counting pronouns on the Wikipedia page. Usually accurate, should be combined with F0 measurement or some other kind of confirmation, but it's mostly reliable.
***
**['birthday']**

Birthday of the speaker. Obtained from Wikipedia, overwritten if Europarl MEP page exists.
***
**['age']**

Age of the speaker at the time of the speech. Calculated from birthday and speech_data['time'].
***
**['native']**

Not set, but always true for the samples because we exclude non-native speakers for the corpus.
***
**['wiki']**

Wikipedia link (and europarl MEP link if it exists).
***
**['ed_name']**

The name of the speaker as provided by europarl-direct.
***
**['ed_aff']**

The political party of the speaker as provided by europarl-direct.
***
**['eu_name']**

The name of the speaker as provided by europarl.
***
**['eu_aff']**

The political party of the speaker as provided by europarl.
***
## language_data
Language data was obtained in a number of steps. First there was the automatic transcription step. Performed once with Kaldi (by phrase) and once with Whisper (whole speech, timestamps provided). The Kaldi transcription was applied to speeches cut into phrases, the Whisper transcription was applied to speech-windows defined by edge-silence removal using [pydub]() and speaker diarization and embeddings using [pyannote](). These transcriptions were then matched with verbatim reports obtained directly from the European Parliament to link them with speaker metadata. 

In a second step these transcriptions were handed to human annotators for each language to proofread, cut the edges, correct whisper and annotate interpreter errors. (Present time, the following is unfinished! It's written in past tense because I don't want to write this again.) After this lengthy process, the corrected transcriptions were time aligned with the Montreal Forced Aligner as this would provide phoneme level alignment and the highest accuracy. The results were transformed into a human-readable dataframe with each row providing a word, start time, end time, duration, average F0, average intensity (compensated for fricatives), a talking speed statistic and (longer) breaks. 

In the last step the words with semantic content were given an index which relates them to the other languages' dataframe's words. This was performed via an expensive process starting with stop word removal and ending with matching words within a word number based scanning window using a comparison of a list of possible translations. Details provided below.
***
**['df_transcription']**

The result of the Kaldi transcription. This is a dataframe with one phrase, eaf ID and time information per row.
***
**['df_confidence']**

Fitting the df_transcription this is a dataframe which holds the Kaldi confidence value, its eaf ID and matching transcription ID.
***
**['df_is_translation']**

Fitting the df_transcription this is a dataframe which holds a Kaldi decision whether the phrase is translated or not, its eaf ID and matching transcription ID.
***
**['df_manually_corrected']**

Fitting the df_transcription this is a dataframe which holds a segment corresponding to the transcription which determines whether the segment has been manually currectes, its eaf ID and matching transcription ID.
***
**['relay_interp']**

not set
***
**['retour_interp']**

not set
***
**['is_translation_perc']**

Calculated from the phrase-wise "is_translation" decision made by Kaldi. This sums the segments weighted by their length, then divides by overall length.
***
**['speech_duration']**

This is calculated from df_transcription time columns, used for Kaldi calculations.
***
**['verbatim_file']**

europarl-direct file provided by Kaldi transcription sequence matched with europarl-direct speeches.
***
**['verbatim_speech']**

europarl-direct speech provided by Kaldi transcription sequence matched with europarl-direct speeches.
***
**['verbatim_ratio']**

sequence matcher ratio provided by Kaldi transcription sequence matched with europarl-direct speeches.
***
**['whisper']**

Transcription by whisper using the following parameters:
- hallucination_silence_threshold = 2, 
- beam_size = 5, 
- patience = 2

These settings have been thoroughly tested on an initial sample with future human annotation in mind. These can (and will be) greatly refined for each language to expand the human annotated sample.
***
**['interpreter_window']**
A tuple containing
- number of speakers not in _1_und
- their index in the diarization and embeddings
- the embeddings of these speakers
- sum of diarized segments
- start of first diarized segment, end of last diarized segment, duration

This is currently used to save a bit of time with the Whisper transcriptions and avoid errors at the edges of a speech. This generally works very well with interpreter channels, less well with the original audio (use longest speaker instead) and unfortunately, but obviously fails if an orator and a previous (or following) speaker have the same language translated by the same interpreter. None of this results in a catastrophic failure (nothing is cut, only too much is included), but the quality of the whisper transcription suffers at the edges.
***
**['w_verbatim_speech']**

***
**['w_verbatim_ratio']**

***
**['w_verbatim_file']**

***
**['p_verbatim_speech']**

***
**['ed_file']**

***
**['ed_ratio']**

***
**['ed_speech']**

***
**['eu_file']**

***
**['eu_ratio']**

***
**['eu_speech']**

***
## file_data
***
**['mp3_paths'']**

***       
**['eaf_path'']**

***
**['folder_name'']**

***
**['file_name'']**

***
## eaf_Data
***

**['time_dictionary'']**

***
**['eaf_duration'']**

***
**['date'']**

***
**['property_tag'']**

***

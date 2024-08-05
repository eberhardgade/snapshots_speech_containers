# Top level Structure
Each container has, 5 attributes: 5 dictionaries.
Each dictionary holds a specific type of information:

![Overview](github-documentation/corpus-structure-top-level.svg)




## speech_data

    self.speech_data    'samples' :                 number of samples in the recording
                        'sample_rate' :             sample rate of recording
                        'length' :                  length in seconds of the recording
                        'orig_lang' :               (not yet set)
                        'orig_lang_kaldi' :         original language as detected by kaldi
                        'kaldi_data' :              data for kaldi decision
                        'orig_lang_zcr' :           original language as determined by zcr-comparison
                        'zcr_data' :                data for zcr decision
                        'orig_lang_whisper' :       original language as determined by whisper
                        'whisper_data' :            data for whisper decision       
                        'orig_lang_sub' :           original language determined by subtracting channels
                        'sub_data' :                data for subtraction decision
                        'diarization:' :            speaker diarization from pyannote
                        'window' :                  most likely interpreter speaking window
                        'date' :                    date of the session (sometimes =/= date of the speech)
                        'time' :                    time of the speech
                        'session' :                 number of the session (1-96)
                        'location' :                location of the session (Strasbourg or Brussels)
                        'cycle' :                   number in cycle (4 in Strasbourg, 2 in Brussels)
                        'subject' :                 topic the speech belongs to
                        'chair' :                   person presiding during the speech
                        'orig_w_verbatim_langs' :   original language according to europarl-scrape matching
                        'orig_lang_ed':             original language according to europarl-direct matching
                        'orig_lang_eu' :            original language according to europarl matching




## speaker_data

    self.speaker_data 'name' :     set_speaker_data() requires europarl-direct or similar info and outputs from meps meta (currently manually set from cleaned resoup data)
                      'group' :    set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'gender' :   set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'birthday' : set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'age' :      set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'native' :   set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'wiki' :     set_speaker_data() requires europarl-direct or similar info and outputs from meps meta
                      'ed_name': based on europarl direct.
                      'ed_aff': based on europarl direct
                      'eu_name': based on europarl
                      'eu_aff' : based on europarl
                      
                      
                      
                      
## language_data

    language_data : 'df_transcription': 
                    'df_confidence': 
                    'df_is_translation': 
                    'df_manually_corrected':  
                    'relay_interp' : none
                    'retour_interp': none
                    'is_translation_perc' : kaldi
                    'speech_duration' : based on samples and sample rate
                    'verbatim_file' : verbatim file according to kaldi sm
                    'verbatim_speech' : 
                    'verbatim_ratio' : 
                    'whisper' : whisper transcription
                    'interpreter_window' : based on diarisation and embedding
                    'w_verbatim_speech':
                    'w_verbatim_ratio':
                    'w_verbatim_file': verbatim file according to whisper sm
                    'p_verbatim_speech': parallelised
                    'ed_file': based on europarl direct.
                    'ed_ratio' : based on europarl direct.
                    'ed_speech' : based on europarl direct.
                    'eu_file'   : based on europarl.
                    'eu_ratio'  : based on europarl.
                    'eu_speech' : based on europarl.
                      
## file_data

    self.file_data  'mp3_paths' :       
                    'eaf_path' :        eaf_extraction[0]
                    'folder_name' :     eaf_extraction[0][76:-4]
                    'file_name':        current_container.file_data['folder_name'][-26:]
                    
                    
File Data holds information about the mp3, eaf, folder and file. This will only work if the whole corpus tree is cloned from GitHub or the corpus structure is reassembled by hand.

## _eaf_data¶

    self._eaf_data    'time_dictionary' : eaf_extraction[1],
                      'eaf_duration' :    max(eaf_extraction[1].values()),
                      'date' :            eaf_extraction[3][0],
                      'property_tag' :    eaf_extraction[3][1]}





# Detailed Overview
## file_data

 '/corpus/{}/'.format(eaf_extraction[0][76:-4])
 
 
 'mp3_paths' :       
                    'eaf_path' :        eaf_extraction[0]
                    'folder_name' :     eaf_extraction[0][76:-4]
                    'file_name':        current_container.file_data['folder_name'][-26:]
## speech_data

    self.speech_data    'samples' :     just set from pickle (requires audio_info_overview folder)
                        'sample_rate' : just set from pickle (requires audio_info_overview folder)
                        'length' :      just set from pickle (requires audio_info_overview folder)
                        'orig_lang' : -------------------
                        'orig_lang_kaldi' : set_orig_lang_kaldi() (requires find_and_set_translation_perc())
                        'kaldi_data' :      set_orig_lang_kaldi (requires find_and_set_translation_perc())
                        'orig_lang_zcr' : set_orig_lang_zcr() (requires pickle from zcr)
                        'zcr_data' :      set_orig_lang_zcr() (requires pickle from zcr)
                        'orig_lang_whisper' :  set_orig_lang_whisper() (requires pickle from whisper)
                        'whisper_data' :       just set from pickle (requires pickle from whisper)
                        'orig_lang_sub' : set_orig_lang_sub() (requires pickle from interpreter_action)
                        'sub_data' :      set_orig_lang_sub() (requires pickle from interpreter_action)
                        'diarization:' : set directly from pydub and pyannote
                        'window' :      set directly from pydub and pyannote
                        'date' :      set_session_data() requires session_list
                        'time' :      set_session_data() requires session_list
                        'session' :   set_session_data() requires session_list
                        'location' :  set_session_data() requires session_list
                        'cycle' :     set_session_data() requires session_list
                        'subject' : set_subject() requires subject-with-recording.pickle --- could be replaced eventually with insanely precise souping.
                        'chair' :   set_chair_from_time() requires chair-changes.pickle --- should be replaced eventually from insanely precise souping.
                        'orig_w_verbatim_langs' : manually set from the cleaned resoup
                        'orig_lang_ed': based on europarl direct.
                        'orig_lang_eu' : based on europarl
## speaker_data
## eaf_Data
## language_data

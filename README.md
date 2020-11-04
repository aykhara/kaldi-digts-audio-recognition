# Digts Audio Recognition with Kaldi

This is a sample code to create a custom model with Kaldi to recognise digits.

## Prerequisites
- Install Kaldi

'''
> git clone https://github.com/kaldi-asr/kaldi.git
> cd kaldi
> cat INSTALL

> cd tools
> cat INSTALL # following the instruction
> extras/check_dependencies.sh # Done if you get a message "all OK."

> nproc # Check CPU core on your environment e.g. 4
> sudo make -j (your CPU core) # replace the core number with yours

> cd ../src
> cat INSTALL # following the instruction
'''

- Install SRILM

'''
Download a file from [the link](http://www.speech.sri.com/projects/srilm/download.html). Then change the file name to "srilm.tgz" and put it under tools/.

> sudo apt-get install -y gawk
> extras/install_srilm.sh
'''

## Check if a sample "yesno" works correctly on your environment
There is an example to recognise yes or no in Hebrew. Just use it as test to see if Kaldi is installed on your environment.

'''
> cd ../egs/yesno/s5
> sh run.sh
...
%WER 0.00 [ 0 / 232, 0 ins, 0 del, 0 sub ] exp/mono0a/decode_test_yesno/wer_10
'''

## Additional prerequisites
- Create a folder "digits" in kaldi/egs/ directly and put all files of this repo in the folder
- Copy two folders - utils and steps from kaldi/egs/wsj/s5 and put them in your kaldi/egs/digits directory

### Directory layout

    egs
    └── digits
        ├── conf                   # Configuration files (it's not necessary to create configuration files)
        ├── data
        ├── digits_audio           # Audio datasets
        ├── utils                  # Necessary Kaldi tools that are widely used in exemplary scripts
        ├── steps                  # Necessary Kaldi tools that are widely used in exemplary scripts
        ├── cmd.sh
        ├── path.sh
        └── run.sh


Now you're ready to run run.sh script.
'''
> cd ../egs/digits
> sh run.sh
'''

## What each folder/file is doing

|folder/file|purpose|
|-|-|
|data/local/corpus.txt|This file contain every single utterance transcription that can occur in your ASR system. In this example it will be 60 lines from 60 audio files.|
|data/local/dict/lexicon.txt|This file contains every word from your dictionary with its 'phone transcriptions'. Pattern: <word> <phone 1> <phone 2> ...|
|data/local/dict/nonsilence_phones.txt|This file lists nonsilence phones that are present in your project.|
|data/local/dict/silence_phones.txt|This file lists silence phones.|
|data/local/dict/optional_silence.txt|This file lists optional silence phones.|
|data/local/score.sh|This script will help you to get decoding results.|
|data/test/spk2gender|This file informs about speakers gender. Currently there is only one male speaker so just use "global" as speaker ID. Pattern: <speakerID> <gender>|
|data/test/wav.scp|This file connects every utterance (sentence said by one person during particular recording session) with an audio file related to this utterance. Pattern: <uterranceID> <full_path_to_audio_file>|
|data/test/text|This file contains every utterance matched with its text transcription. Pattern: <uterranceID> <text_transcription>|
|data/test/utt2spk|This file tells the ASR system which utterance belongs to particular speaker. Pattern: <uterranceID> <speakerID>|
|data/train|Same as test folder structure|
|digits_audio/test|test datasets - 10% of all wav data|
|digits_audio/train|training datasets - 90% of all wav data|
|utils|Necessary Kaldi tools that are widely used in exemplary scripts.|
|steps|Necessary Kaldi tools that are widely used in exemplary scripts.|
|cmd.sh|Setting local system jobs|
|path.sh|Setting path|
|run.sh|Running script|

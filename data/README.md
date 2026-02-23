## Pre-training data (AudioSet)

The pre-trainer (e.g., `train_audio.py` for audio) loads data from the `data` folder by default (`--data_path`), using a list of samples in a CSV  file `data/files_audioset.csv` by default (`--dataset`).

The CSV file should have a `file_name` column containing the relative pathname of the files containing a log-mel spectrogram (LMS) audio. Example:

```
file_name
audioset_lms/balanced_train_segments/--aE2O5G5WE_0.000.npy
audioset_lms/balanced_train_segments/--cB2ZVjpnA_30.000.npy
audioset_lms/balanced_train_segments/--aaILOrkII_200.000.npy
audioset_lms/balanced_train_segments/--ZhevVpy1s_50.000.npy
audioset_lms/balanced_train_segments/--aO5cdqSAg_30.000.npy
audioset_lms/balanced_train_segments/--PJHxphWEs_30.000.npy
audioset_lms/balanced_train_segments/--ekDLDTUXA_30.000.npy
```

The folders/files should look like the following:

    (Example of the folder structure)
    data/
        audioset_lms/
            balanced_train_segments/
                --aE2O5G5WE_0.000.npy
                --cB2ZVjpnA_30.000.npy
                  :

If you also have pre-processed FSD50K data, the folder will be as follows:

    (Example of the folder structure)
    data/
        audioset_lms/
          :
        fsd50k_lms/
            FSD50K.dev_audio/
                2931.npy
                408195.npy
                    :

### ✅️ Example preprocessing steps (AudioSet)

If you have downloaded the AudioSet samples and converted them into .wav files in `/your/local/audioset` folder, the following example steps will preprocess and create a new folder, `data/audioset_lms`.

1. Convert your pre-training data to LMS using [`wav_to_lms.py`](../wav_to_lms.py). Example: `python wav_to_lms.py /your/local/audioset data/audioset_lms`
2. Then, make two file lists under your `data` folder. Example follows:

    ```sh
    echo file_name > data/files_audioset.csv
    (cd data && find audioset_lms/balanced_train_segments -name "*.npy") | sort >> data/files_audioset.csv
    (cd data && find audioset_lms/unbalanced_train_segments -name "*.npy") | sort >> data/files_audioset.csv
    echo file_name > data/files_audioset_eval.csv
    (cd data && find audioset_lms/eval_segments -name "*.npy") | sort >> data/files_audioset_eval.csv
    ```

The `files_audioset.csv` contains training files, whereas `files_audioset_eval.csv` contains validation files.

### ✅️ Example preprocessing steps (VGGSound)

If you have downloaded the AudioSet samples and converted them into .wav files in `/your/local/VGGSound` folder, the following step will preprocess the data and create a new folder, `data/vggsound_lms`.

1. Convert the pre-training data to LMS using [`wav_to_lms.py`](../wav_to_lms.py). Example (for MP4 files):

```sh
python wav_to_lms.py /your/local/VGGSound data/vggsound_lms --suffix .mp4
```


### ✅️ Example preprocessing steps (WavCaps)

You can download files from [huggingface.co/datasets/cvssp/WavCaps](https://huggingface.co/datasets/cvssp/WavCaps).
Please make sure you have the following:

    your/local/WavCaps/
        AudioSet_SL_flac  BBC_Sound_Effects_flac  FreeSound_flac  SoundBible_flac

The following step will preprocess the data and create a new folder, `data/wavcaps_lms`.

```sh
python wav_to_lms.py your/local/WavCaps data/wavcaps_lms --suffix .flac
```

### ✅️ Example preprocessing steps (Clotho)

You can download files from [Zenodo](https://zenodo.org/records/4783391).
Please make sure you have the following:

    your/local/clotho/
        development/
            Ambience Birds.wav
            :
        validation/
            :
        evaluation/
            :

The following step will preprocess the data and create a new folder, `data/clotho_lms`.

```sh
python wav_to_lms.py your/local/clotho data/clotho_lms
```

The `development` split is used in Stage 2 of M2D-CLAP pre-training. The file list is created as part of the [M2D-CLAP setup](../clap/README.md#1-setup) (Step 3: Run `Generate-File-Lists.ipynb`).

### ✅️ Example preprocessing steps (AudioCaps)

The files are available upon request. Fill out the form linked from the [AudioCaps dataset README](https://github.com/cdjkim/audiocaps/blob/master/dataset/README.md) and the maintainers will send you the download link. Once you have extracted the files, please make sure you have the following:

    your/local/audiocaps/
        train/
            Y*.wav
            :
        val/
            :
        test/
            :

The following step will preprocess the data and create a new folder, `data/audiocaps_lms`.

```sh
python wav_to_lms.py your/local/audiocaps data/audiocaps_lms
```

The `train` split is used in Stage 2 of M2D-CLAP pre-training. The file list is created as part of the [M2D-CLAP setup](../clap/README.md#1-setup) (Step 3: Run `Generate-File-Lists.ipynb`).

### ✅️ Example preprocessing steps (FSD50K)

The following example will create files: `files_f_s_d_5_0_k.csv` for training files and `files_fsd50k_eval.csv` for validation files.

    ```sh
    echo file_name > data/files_f_s_d_5_0_k.csv
    (cd data && find fsd50k_lms/FSD50K.dev_audio -name "*.npy") | sort >> data/files_f_s_d_5_0_k.csv
    echo file_name > data/files_fsd50k_eval.csv
    (cd data && find fsd50k_lms/FSD50K.eval_audio -name "*.npy") | sort >> data/files_fsd50k_eval.csv
    ```

### ✅️ Example preprocessing steps (ICBHI2017)

The following example will create files: `files_icbhi2017.csv` for training files and `files_fsd50k_eval.csv` for validation files.

    ```sh
    echo file_name > data/files_icbhi2017.csv
    (cd data && find icbhi2017_lms/train -name "*.npy") | sort >> data/files_icbhi2017.csv
    echo file_name > data/files_icbhi2017_eval.csv
    (cd data && find icbhi2017_lms/val -name "*.npy") | sort >> data/files_icbhi2017_eval.csv
    ```

### ✅️ Example preprocessing steps (SPRSound)

The following example will create files: `files_sprs.csv` for training files and `files_sprs_eval.csv` for validation files.

    ```sh
    echo file_name > data/files_sprs.csv
    (cd data && find sprsound_lms/train -name "*.npy") | sort >> data/files_sprs.csv
    echo file_name > data/files_sprs_eval.csv
    (cd data && find sprsound_lms/val -name "*.npy") | sort >> data/files_sprs_eval.csv
    ```

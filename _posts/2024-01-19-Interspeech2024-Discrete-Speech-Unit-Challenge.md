---
layout: post
title: Interspeech2024 Speech Processing Using Discrete Speech Unit Challenge
description: This is the Interspeech2024 challenge website for speech processing using discrete speech unit challenge
date: 2024-01-19 09:00:00-0800
comments: false
---

<!---
### The Challenge Overview

Description here
--->

## Introduction

In conventional speech processing approaches, models typically take either raw waveforms or high-dimensional features derived from these waveforms as input. For instance, spectral speech features continue to be widely employed, while learning-based deep neural network features have gained prominence in recent years. A promising alternative arises in the form of discrete speech representation, where speech signals within a temporal window can be represented by a discrete token as shown in this [work](https://arxiv.org/pdf/2309.15800.pdf).

Three challenging tasks are proposed for using discrete speech representations. 
1. Automatic speech recognition (ASR): We will evaluate the ASR performance of the proposed systems on the proposed data.
2. Text-to-speech (TTS): We will evaluate the quality of the generated speech.
3. Singing voice synthesis (SVS): We will evaluate the quality of the synthesized singing voice.


Participation is open to all. Each team can participate in any task. This challenge has preliminarily been accepted as a special session for Interspeech 2024, and participants are strongly encouraged to submit papers to the special session. The focus of the special session is to promote the adoption of discrete speech representations and encourage novel insights.

<!---
### Resources
--->

## Resources


### Baseline systems & toolkits
- [Automatic speech recognition (ASR)](https://github.com/espnet/espnet/tree/master/egs2/interspeech2024_dsu_challenge/asr2)
  * Results
    * WER is computed on English test sets (dev-clean / dev-other / test-clean / test-other)
    * CER is computed on the multi-lingual test set (test_1h)
<table class="table">
  <thread>
    <tr>
      <th scope="col">Model</th>
      <th scope="col">dev-clean (LS)</th>
      <th scope="col">dev-other (LS)</th>
      <th scope="col">test-clean (LS)</th>
      <th scope="col">test-other (LS)</th>
      <th scope="col">test-1h (ML-SUPERB)</th>
    </tr>
  </thread>
  <tbody>
    <tr>
      <th scope="col">Wavlm-large-layer21</th>
      <th scope="col">4.5</th>
      <th scope="col">8.1</th>
      <th scope="col">4.4</th>
      <th scope="col">8.3</th>
      <th scope="col">72.6</th>
    </tr>
  </tbody>
</table>
- [Text-to-speech (TTS)](https://github.com/espnet/espnet/tree/master/egs2/ljspeech/tts2)
  * Results
<table class="table">
  <thread>
    <tr>
      <th scope="col">Model</th>
      <th scope="col">MCD</th>
      <th scope="col">Log F0 RMSE</th>
      <th scope="col">WER</th>
      <th scope="col">UTMOS</th>
    </tr>
  </thread>
  <tbody>
    <tr>
      <th scope="col">HuBERT-base-layer6</th>
      <th scope="col">7.19</th>
      <th scope="col">0.26</th>
      <th scope="col">8.1</th>
      <th scope="col">3.73</th>
    </tr>
  </tbody>
</table>
- [Singing voice synthesis (SVS)](https://github.com/A-Quarter-Mile/espnet/tree/tmp_muskit/egs2/opencpop/svs2)
<table class="table">
  <thread>
    <tr>
      <th scope="col">Model</th>
      <th scope="col">MCD</th>
      <th scope="col">Log F0 RMSE</th>
    </tr>
  </thread>
  <tbody>
    <tr>
      <th scope="col">WavLM-large-layer6</th>
      <th scope="col">8.47</th>
      <th scope="col">0.18</th>
    </tr>
  </tbody>
</table>
- [Discrete vocoder training](https://github.com/kan-bayashi/ParallelWaveGAN)
<table class="table">
  <thread>
    <tr>
      <th scope="col">Model</th>
      <th scope="col">MCD</th>
      <th scope="col">Log F0 RMSE</th>
      <th scope="col">UTMOS</th>
    </tr>
  </thread>
  <tbody>
    <tr>
      <th scope="col">HuBERT-base-layer6</th>
      <th scope="col">7.19</th>
      <th scope="col">0.42</th>
      <th scope="col">2.27</th>
    </tr>
  </tbody>
</table>


### Track-specific dataset

- ASR: [Librispeech](https://www.openslr.org/12) and [ML-SUPERB](https://drive.google.com/file/d/1zslKQwadZaYWXAmfBCvlos9BVQ9k6PHT/view?usp=sharing)
- TTS: [LJSpeech](https://keithito.com/LJ-Speech-Dataset/) and [Expresso](https://speechbot.github.io/expresso/)
- SVS: [Opencpop](https://wenet.org.cn/opencpop/)


### Data for discrete representation learning and extraction
- General Policy: There are no restrictions on using datasets for learning and extracting discrete representations. This applies broadly to all datasets.

- Specific Restrictions for Supervision Data: The key restriction is on using test sets from certain datasets for supervision in specific tasks. Specifically:
  - Automatic Speech Recognition (ASR): The test sets of the Librispeech and ML-SUPERB datasets cannot be used for learning the discrete representation. However, their training sets are permissible.
  - Text-to-Speech (TTS): The test sets of the LJSpeech and Expresso datasets are off-limits for discrete representation learning, but their training sets can be used. For TTS training, phone alignment information for non-autoregressive training can be also used in the training phase.
  - Singing Voice Synthesis (SVS): The test set of the Opencpop dataset is restricted for use in discrete representation learning, though the training set is allowed.

<!-- ### Rules
* For each task, the training data must follow the baseline systems. However, there is no constraint on the data used in the foundation models.
* For submission, more details will be provided later for each task.
 -->

## Detailed tracks and rules

### ASR Challenge

* Data: LibriSpeech_100 + ML-SUPERBB 1h set
* Framework: We recommend to use ESPnet for fair comparison. Feel free to let us know your preferrence.
* Evaluation metrics: 1) Character Error Rates (CERs) on LibriSpeech and ML-SUPERB evaluation sets; 2) the bitrate of the discrete unit.
  * Character Error Rate (CER): This metric measures the performance of a system in terms of the accuracy of the words recognized or generated compared to a reference. All systems are ranked based on the CERs of the evaluation sets separately: 1) EN: dev-clean / dev-other / test-clean / test-other; 2) ML: test-1h. Note that the ranking of the EN case is based on the micro-average CER of all Librispeech test sets (i.e., total errors of {dev,test}-{clean,other}) / (total length of {dev,test}-{clean,other}).
  * Bitrate of the discrete unit: This metric measures the efficiency of the discrete representation. Considering different sequence reduction methods (e.g., BPE, deduplication, etc.), we estimate bitrate by considering the whole test set (i.e., all librispeech evaluation sets and ML-SUPERB test sets). Denote the discrete token as $$\{\mathbf{S}_1, ..., \mathbf{S}_M\}$$ where $$\mathbf{S}_i$$ is the $$i^{\text{th}}$$ stream of discrete token and $$M$$ is the number of streams. We then define the vocabulary size of $$i^{\text{th}}$$ stream as $$V_i$$ and the length of $$i^{\text{th}}$$ stream as $$L_i$$. Considering the total length of test sets as $$N$$ (in seconds), the bitrate of the discrete token $$B$$ is defined as $$B = \sum_{i=1}^M(L_i / N * \text{log}_2(V_i))$$
* Ranking: The overall ranking is based on the Average Rank, which is the average of all three ranking positions:
  * R1: micro average CER on all LibriSpeech evaluation sets;
  * R2: CER on ML-SUPERB test set;
  * R3: the bitrate of the overall test sets.

  The overall ranking position is (R1 + R2 + R3) / 3. If more than 1 systems have the same overall ranking position, they are further ranked by the CER of the test-1h (Please see FAQ section for a detailed example of the ranking).
* Submission package details:
  1. The vocabulary file of input, which includes all possible input token types and special tokens (`<sos>`, `<eos>`, `<blank>`, etc) within a json format. The key is the order of input streams, while the value is the token list corresponding to the key. Example out:
    ```
    {
      0: ["0", "1", "2", ...],
      1: ["100", "101", "102", ...],
      ...
    }
    ```

    In the case of baseline, you may convert the vocab file at `data/token_list/src_bpe_unigram3000_rm_wavlm_largge_21_km2000/bpe.vocab`  to an example output like:
    ```
    {
      0: ["<unk>", "<s>", "</s>", "么", "喤", "佨", "叡", ...]
    }
    ```

  2. The input discrete speech units corresponding to the test sets in a json format. The key of the submission file is the utterance id (refer to ESPnet recipe), the value is a two-dimensional list, with each list corresponds to a stream of discrete tokens.

     E.g. if you follow the baseline and use bpe, the input file can be derived by `paste <(cut -f1 -d" " dump/raw/test_1h/text.rm.wavlm_large_21_km2000) <(spm_encode --model=data/token_list/src_bpe_unigram3000_rm_wavlm_large_21_km2000/bpe.model --output_format=id <dump/raw/test_1h/text.rm.wavlm_large_21_km2000) > output`. Example output (baseline):
    ```
    {
      "AccentedFrenchOpenSLR57_fra_000005": [[784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600]]
    }
    ```

    If you are using multiple streams, the other streams would be in additional list. Example output:
    ```
    {
      "AccentedFrenchOpenSLR57_fra_000005": [
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
          ]
    }
    ```
    Noted that the token number in each stream is not necessararily the same to each other (e.g., the first stream may have a resolution of 20ms, but the second may be 40ms etc.)

  3. The predicted transcription corresponding to the test sets.
  4. A technical report in Interspeech2024 paper format (no length limit, can be submitted one week after the Interspeech submission deadline (i.e., 2024/03/18 AOE))


### TTS Challenge - Acoustic+Vocoder

* Data: LJSpeech, following the train-dev-test split in [here](https://github.com/ftshijt/Interspeech2024_DiscreteSpeechChallenge).
* Framework: No framework or model restriction in the TTS-Acoustic+Vocoder challenge, but the organizers have prepared the baseline training scripts (baseline model to be released soon) in [ESPnet](https://github.com/espnet/espnet/tree/tts2/egs2/ljspeech/tts2).
* Evaluation metrics: Mean cepstral distortion, F0 root mean square error, Bitrate, [UTMOS](https://github.com/sarulab-speech/UTMOS22/tree/master)
  * Bitrate of the discrete unit: This metric measures the efficiency of the discrete representation. Considering different sequence reduction methods (e.g., BPE, deduplication, etc.), we estimate bitrate by considering the LJSpeech test set (according to the official split provided in the challenge). Denote the discrete token as $$\{\mathbf{S}_1, ..., \mathbf{S}_M\}$$ where $$\mathbf{S}_i$$ is the $$i^{\text{th}}$$ stream of discrete token and $$M$$ is the number of streams. We then define the vocabulary size of $$i^{\text{th}}$$ stream as $$V_i$$ and the length of $$i^{\text{th}}$$ stream as $$L_i$$. Considering the total length of test sets as $$N$$ (in seconds), the bitrate of the discrete token $$B$$ is defined as $$B = \sum_{i=1}^M(L_i / N * \text{log}_2(V_i))$$
* Ranking: The overall ranking is based on the Average Rank, which is the average of two ranking positions:
  * R1: UTMOS;
  * R2: the bitrate of the overall test sets.

  The overall ranking position is (R1 + R2) / 2. If more than 1 systems have the same overall ranking position, they are further ranked by R1 (Please see FAQ section for a detailed example of the ranking).
* Submission
   * Submission package details:
     1. The synthesized voice of LJSpeech test set using full training set (with at least 16kHz, in zipped format).
     2. The synthesized voice of LJSpeech test set using 1-hour training set (with at least 16kHz, in zipped format).
     3. The input discrete speech units corresponding to the test set in a json format. The key of the submission file is the utterance id (refer to the ID in data-split repo), the value is a two-dimensional list, with each list corresponds to a stream of discrete tokens.

     Example output (baseline):
    ```
    {
      "LJ050-0029": [[784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600]]
    }
    ```

    If you are using multiple streams, the other streams would be in additional list. Example output:
    ```
    {
      "LJ050-0029": [
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
          ]
    }
    ```
    Noted that the token number in each stream is not necessararily the same to each other (e.g., the first stream may have a resolution of 20ms, but the second may be 40ms etc.)
     4. A technical report in Interspeech2024 paper format (no length limit, can be submitted one week after the Interspeech submission deadline (i.e., 2024/03/18 AOE))



### TTS Challenge - Vocoder

* Data: Expresso, following the train-dev-test split in [here](https://github.com/ftshijt/Interspeech2024_DiscreteSpeechChallenge) (Note that this is different from the original train-dev-test split in the benchmark paper).
* Framework: No framework or model restriction in TTS-Vocoder challenge, but the organizers have prepared the baseline training scripts (baseline model to be released soon) in [ESPnet](https://github.com/espnet/espnet/tree/tts2/egs2/ljspeech/tts2) and [ParallelWaveGAN](https://github.com/kan-bayashi/ParallelWaveGAN).
* Evaluation metrics: Mean cepstral distortion, F0 root mean square error, Bitrate, [UTMOS](https://github.com/sarulab-speech/UTMOS22/tree/master)
  * Bitrate of the discrete unit: This metric measures the efficiency of the discrete representation. Considering different sequence reduction methods (e.g., BPE, deduplication, etc.), we estimate bitrate by considering the Expresso test set (according to the official split provided in the challenge). Denote the discrete token as $$\{\mathbf{S}_1, ..., \mathbf{S}_M\}$$ where $$\mathbf{S}_i$$ is the $$i^{\text{th}}$$ stream of discrete token and $$M$$ is the number of streams. We then define the vocabulary size of $$i^{\text{th}}$$ stream as $$V_i$$ and the length of $$i^{\text{th}}$$ stream as $$L_i$$. Considering the total length of test sets as $$N$$ (in seconds), the bitrate of the discrete token $$B$$ is defined as $$B = \sum_{i=1}^M(L_i / N * \text{log}_2(V_i))$$
* Ranking: The overall ranking is based on the Average Rank, which is the average of two ranking positions:
  * R1: UTMOS;
  * R2: the bitrate of the overall test sets.

  The overall ranking position is (R1 + R2) / 2. If more than 1 systems have the same overall ranking position, they are further ranked by R1.
* Submission
   * Submission package details:
     1. The synthesized voice of Expresso test set using full training set (with at least 16kHz, in zipped format).
     2. The input discrete speech units corresponding to the test set in a json format. The key of the submission file is the utterance id (refer to the ID in data-split repo), the value is a two-dimensional list, with each list corresponds to a stream of discrete tokens.

     Example output (baseline):
    ```
    {
      "ex01_confused_00001": [[784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600]]
    }
    ```

    If you are using multiple streams, the other streams would be in additional list. Example output:
    ```
    {
      "ex01_confused_00001": [
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
          ]
    }
    ```
    Noted that the token number in each stream is not necessararily the same to each other (e.g., the first stream may have a resolution of 20ms, but the second may be 40ms etc.)
     3. A technical report in Interspeech2024 paper format (no length limit, can be submitted one week after the Interspeech submission deadline (i.e., 2024/03/18 AOE))



### SVS Challenge

* Data: Opencpop, following the original segmentation and train/test split.
* Framework: No framework or model restriction in the SVS challenge, but the organizers have prepared the baseline training scripts (baseline model to be released soon) in [ESPnet-Muskits](https://github.com/A-Quarter-Mile/espnet/tree/tmp_muskit/egs2/opencpop/svs2).
* Evaluation metrics
   * Objective metrics: Mean cepstral distortion, F0 root mean square error, Bitrate for efficiency measure
     * Bitrate of the discrete unit: This metric measures the efficiency of the discrete representation. Considering different sequence reduction methods (e.g., BPE, deduplication, etc.), we estimate bitrate by considering the Opencpop test set (according to the official split provided in the challenge). Denote the discrete token as $$\{\mathbf{S}_1, ..., \mathbf{S}_M\}$$ where $$\mathbf{S}_i$$ is the $$i^{\text{th}}$$ stream of discrete token and $$M$$ is the number of streams. We then define the vocabulary size of $$i^{\text{th}}$$ stream as $$V_i$$ and the length of $$i^{\text{th}}$$ stream as $$L_i$$. Considering the total length of test sets as $$N$$, the bitrate of the discrete token $$B$$ is defined as $$B = \sum_{i=1}^M(L_i / N * \text{log}_2(V_i))$$
   * Subjective metrics: Mean Opinion Score (MOS) by organizers
* Ranking: The overall ranking is based on the Average Rank, which is the average of two ranking positions:
  * R1: MOS;
  * R2: the bitrate of the overall test sets.

  The overall ranking position is (R1 + R2) / 2. If more than 1 systems have the same overall ranking position, they are further ranked by R1 (Please see FAQ section for a detailed example of the ranking).
* Submission
   * Submission package details:
     1. The synthesized voice of Opencpop test set using full training set (with at least 16kHz, in zipped format).
     2. The input discrete speech units corresponding to the test set in a json format. The key of the submission file is the utterance id (refer to the ID in data-split repo), the value is a two-dimensional list, with each list corresponds to a stream of discrete tokens.

     Example output (baseline):
    ```
    {
      "ex01_confused_00001": [[784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600]]
    }
    ```

    If you are using multiple streams, the other streams would be in additional list. Example output:
    ```
    {
      "ex01_confused_00001": [
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
              [784, 0, 1953, 1126, 9, 1126, 547, 273, 443, 541, 16, 768, 10, 806, 1380, 1151, 61, 382, 1004, 765, 2162, 1698, 128, 2621, 357, 914, 480, 715, 89, 1369, 893, 1307, 266, 64, 266, 681, 828, 641, 689, 1026, 488, 448, 182, 860, 1552, 628, 233, 1156, 22, 438, 659, 2239, 1125, 888, 22, 888, 1493, 752, 283, 123, 1296, 266, 64, 1000, 1187, 548, 1481, 671, 318, 629, 652, 89, 312, 1451, 88, 1826, 504, 1588, 145, 1296, 266, 64, 542, 340, 1805, 651, 217, 962, 1519, 229, 10, 403, 600],
          ]
    }
    ```
    Noted that the token number in each stream is not necessararily the same to each other (e.g., the first stream may have a resolution of 20ms, but the second may be 40ms etc.)
    3. A technical report in Interspeech2024 paper format (no length limit, can be submitted one week after the Interspeech submission deadline (i.e., 2024/03/18 AOE))



### Research in the discrete representation of speech and audio

* Call for research papers: As a special session, the track also accepts research papers in discrete representation of speech and audio. The paper could be related to any of the following topics:
  * Discrete speech/audio/music representation learning
  * Discrete representation application for any speech/audio processing downstream tasks (ASR, TTS, etc.)
  * Evaluation of speech/audio discrete representation
  * Efficient discrete speech/audio discrete representation
  * Interpretability in discrete speech/audio discrete representation
  * Other novel usage of discrete representation in speech/audio
* Please refer to the "Paper submission" section for detailed guidance on paper submission.


##  Paper submission

Papers for the Interspeech Special Session have to be submitted following the same schedule and procedure as regular papers of [INTERSPEECH 2024](https://interspeech2024.org/). The submitted papers will undergo the same review process by anonymous and independent reviewers. 

Please use the [submission URL](https://cmt3.research.microsoft.com/INTERSPEECH2024)) to submit papers and select "14.10 Speech Processing Using Discrete Speech Units (Special Session)" as the primary subject areas for your paper submission.

For techincal reports, please submit the paper via the google form: https://forms.gle/K7fehBcoVEMXB9tx9

##  Submission pakcage submission

Participants will need to submit their submission package through the [google form](https://forms.gle/XAdQ5WyEVD2tDwYh8), where the leaderboard will be updated accordingly within 48 hours. The final ranking information will be recorded at the time of Interspeech submission deadline (i.e., 2024/03/11 AOE).

Each team will only be able to submit up-to 5 systems to the leaderboard.

For SVS track, because of the limited budget and the requirements of extra evaluation time for the subjective evaluation, up-to 3 systems are accepted to the leaderboard and deadline for final submissions is 2024/03/05 AOE. While the bitrate evaluation in the leaderboard will be updated within 48 hours, the MOS leaderboard will be updated before 2024/03/10 AOE.

The leaderboard is online at https://huggingface.co/discrete-speech.


<!---
### Schedules
--->

## Important dates
The schedule for the challenge is as follows
* Feb 20, 2024: [Leaderboard](https://huggingface.co/discrete-speech) is online and accepting submissions
* Mar  2, 2024: Paper Submission Deadline
* Mar  11, 2024: Paper Revision Deadline
* After Mar. 11: The Leaderboard will still be open and new submissions will be evaluated


## FAQ
-  For each track, you have shown a train set. Is the data used for each track limited to those datasets? Or can we use other datasets such as librilight. If the dataset used for training is limited to the one shown on the website, can we use pretrained models such as Whisper or llama2?
    - For discrete representation/units extraction, we do not have requirements of the data to use, so you may use any of the models you mentioned such as Librilight, or pre-trained models such as Whisper or Llama2. (But to make sure that the supervision leakage, we do not allow you to use supervision data in the track test data; For example, you cannot use Librispeech test data and ML-SUPERB test data with their labels for discrete representation extraction purposes.)
- Can we use additional information such as text/phoneme sequence for vocoders in TTS tracks?
    - For the TTS (acoustic+vocoder) track, you can use text/phoneme sequence in the vocoder. However, for the TTS (vocoder) track, you can only use discrete representations, where the discrete representation can be only extracted from the waveform.
- Can we use additional information such as phone, duration, and note sequences for vocoders in the SVS track
    - Yes, you can use the music score information in the vocoder of SVS systems.
- Can you provide the evaluation scripts of the TTS/SVS objective metrics?
    - We will use the scripts in https://github.com/espnet/espnet/tree/master/egs2/TEMPLATE/tts1#evaluation for objective metrics.
- What does it mean for "No framework or model restrictions in TTS/SVS"?
    - As we offered baselines in ESPnet, we do not have any requirements for only using ESPnet. In short, you may use any toolkits (e.g., coqui-TTS, speechbrain, etc.) or any models (Tacotron, Fastspeech, diffusion-based models, decoder-only AR models such as Vall-E or spearTTS) for the purpose, as long as you follow the other guidelines in the challenge.
- 'Submission package details' in the TTS vocoder challenge says "with at least 16kHz". However, the Expresso dataset is 48 kHz. Should I conduct experiments at 48kHz, or is it acceptable to conduct experiments at any sampling rate greater than 16kHz?
    - For target audio, we will do a resample to 16kHz if participants submit >16kHz audio (that is mostly because the evaluation metrics (e.g., WER/UTMOS) are performed on 16kHz audio-only.
- Will the organizers also consider other metrics for the evaluation (especially for TTS and SVS)?
    - We may add additional metrics for participants' reference. However, we will stick to the current ranking metrics for now to keep it fair for all the participants.
- Can the participants use additional information for training the TTS acoustic model (such as use Mel spectrogram to train VAE or duration information to train fastspeech-like models)?
   - Yeah, additional information from the audio can be used for the TTS acoustic model as long as the output of TTS acoustic model is in discrete space.
- Do you have an example of the rankings?
    - Take ASR as an example, if system A ranks 1st place in R1, 2nd place in R2, 3rd place in R3; system B ranks 3rd place in R1, 1nd place in R2, 2nd place in R3, the overall ranking positions for system A and B are both `2`. However, considering the rank in R2, system B would have a better final ranking.
- Why there are two deadlines for the paper submission, what are their differences?
    - We have two kinds of submission available, which you may select from:
        - Submit to Interspeech as a regular paper (for the special session, need to select discrete speech challenge as the primary topic):
            - for this option, the deadline is the same as Interspeech paper. Noted that the reviewing process is the same as the regular Interspeech paper (and will be in the proceedings as regular paper). And you can include this submission as your system description paper for the challenge summary in the submission package via google form. If you select this option, the deadline is March 2, 2024 for abstract and revision deadline is March 11, 2024.
        - Submit via google form as the system description paper:
            - For this option, you will do not need to submit to the Interspeech portal, but just include your system description paper in the google form (noted that the paper will not be in the proceedings, in other words, no peer review). This paper will be used for the challenge summary (selected system description papers might be provided chances to be presented in the special session). If you select only this option, the deadline is March 18, 2024.
    

<!---
### Organizers
--->

## Organizers

* Xuankai Chang (Carnegie Mellon University, U.S.)
* Jiatong Shi (Carnegie Mellon University, U.S.)
* Shinji Watanabe (Carnegie Mellon University, U.S.)
* Yossi Adi (Hebrew University, Israel)
* Xie Chen (Shanghai Jiao Tong University, China)
* Qin Jin (Renmin University of China, China)

## Contact
- discrete_speech_challenge@googlegroups.com

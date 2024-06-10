---
title: Embracing Video Evaluations with LMMs-Eval Upgrades v0.2.0
date: 2024-06-10
draft: false
language: en
featured_image: ../assets/images/pages/lmms-eval-video.png
summary: We introduce a video evaluation feature to lmms-eval, supporting video model evaluations with over most popular datasets.
# author: TailBliss
# authorimage: ../assets/images/global/author.webp
categories: Blog
tags: video models
---

<p>
  <span style="color: gray; font-size: 16px;">
    <a href="">Kairui Hu</a>
    <a href="https://www.linkedin.com/in/kaichen-zhang-014b17219/?originalSubdomain=sg">Kaichen Zhang</a>, 
    <a href="https://pufanyi.github.io/">Fanyi Pu</a>, 
    <a href="https://zhangyuanhan-ai.github.io/">Yuanhan Zhang</a>, 
    <a href="https://brianboli.com/">Bo Li</a>, 
    <a href="https://liuziwei7.github.io/">Ziwei Liu</a>
  </span>
</p>
<p style="font-size: smaller;">
  <sup>*</sup> indicates equal contribution.
</p>

## Introduction

In the journey towards multimodal intelligence, the development of LMMs has progressed remarkably, transitioning from handling static images to processing complex video inputs. This evolution is crucial, enabling models to understand and interpret dynamic scenes with temporal dependencies, motion dynamics, and contextual continuity. The importance of video evaluation is also increasing across various applications. However, there has been a noticeable absence of comprehensive benchmarks to evaluate the diverse array of video tasks. The introduction of `lmms-eval/v0.2.0` is both necessary and significant as it addresses this critical gap in the evaluation of video-based LMMs.

Building upon the success of `lmms-eval/v0.1.0`, `lmms-eval/v0.2.0` is a comprehensive benchmark designed specifically for video datasets. It ensures fairness by consolidating various benchmarks and evaluations into a single framework, providing consistent results across multiple datasets and tasks. This facilitates the fair comparison of different models using the same standardized benchmark. `lmms-eval/v0.2.0` is capable of running multiple datasets and tasks simultaneously, ensuring a thorough and efficient evaluation process. This framework accelerates the model iteration lifecycle, making it easier for researchers and developers to evaluate the performance of video LMMs.

By providing a fair, comprehensive, and efficient evaluation framework, `lmms-eval/v0.2.0`is set to make a significant impact on the research and development community.

## Video Evaluation in LMMs

### Frame Extraction for Evaluation

In our framework, video evaluation can be viewed as extracting multiple frames to represent a video and then feeding the multi-frame input to the model for inference. This perspective allows us to enhance the zero-shot video understanding capability of image models by utilizing their potential to process sequences of frames as visual tokens. When each frame is considered as part of a concatenated sequence, image-only-trained models like LLaVA-Next can achieve impressive performance on video-related tasks. This highlights the significant advancement in the zero-shot video understanding capability of image models, representing a notable step forward for LMMs.

When defining these models, we also specify the number of frames to be extracted. Extracting more frames typically enhances the model's understanding of the entire video.

Besides, some models like Gemini and Reka do not expose the video processing interface. Our interface accommodates this by directly uploading the raw data files for evaluation.

### Challenges with Audio Integration

One of the most concerning issues with these evaluations is the lack of focus on audio inputs. The majority of current evaluations overlook audio, which is a significant drawback. Audio plays a pivotal role in video content, offering supplementary context and information. At present, only the WorldQA dataset explicitly necessitates audio information to answer questions accurately. This underscores a critical gap in the evaluation process that future frameworks must address to ensure a more comprehensive evaluation of video understanding.

### **Meta Information for Video Datasets**

Table 1: Video Dataset Meta Information

<table style="white-space: nowrap; display: flex; justify-content: center; align-items: center;">
  <tr class="bg-white-100">
    <th class="bg-blue-100 border text-left px-8 py-4">Dataset</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Split</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Task Name</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Task Format</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Evaluation Metric</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Video Source</th>
    <th class="bg-blue-100 border text-left px-8 py-4">Average Length</th>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">ActivityNet-QA</td>
    <td class="border px-8 py-4">Test</td>
    <td class="border px-8 py-4">activitynetqa</td>
    <td class="border px-8 py-4">Open-ended</td>
    <td class="border px-8 py-4">GPT-Eval</td>
    <td class="border px-8 py-4">Internet</td>
    <td class="border px-8 py-4">120s</td>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">EgoSchema</td>
    <td class="border px-8 py-4">Full</td>
    <td class="border px-8 py-4">egoschema</td>
    <td class="border px-8 py-4">MCQ</td>
    <td class="border px-8 py-4">Submission</td>
    <td class="border px-8 py-4">Ego4D</td>
    <td class="border px-8 py-4">180s</td>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">YouCook2</td>
    <td class="border px-8 py-4">Validation</td>
    <td class="border px-8 py-4">youcook2_val</td>
    <td class="border px-8 py-4">MCQ</td>
    <td class="border px-8 py-4" style="white-space: nowrap">Bleu; METEOR; ROUGE_L; CIDEr</td>
    <td class="border px-8 py-4">YouTube</td>
    <td class="border px-8 py-4">311.6s</td>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">Vatex</td>
    <td class="border px-8 py-4">Test</td>
    <td class="border px-8 py-4">vatex_test</td>
    <td class="border px-8 py-4">Caption Matching</td>
    <td class="border px-8 py-4">Accuracy</td>
    <td class="border px-8 py-4">YouTube</td>
    <td class="border px-8 py-4">147.6s</td>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">Vatex-ZH</td>
    <td class="border px-8 py-4">Validation</td>
    <td class="border px-8 py-4">vatex_val_zh</td>
    <td class="border px-8 py-4">Caption Matching</td>
    <td class="border px-8 py-4">Accuracy</td>
    <td class="border px-8 py-4">YouTube</td>
    <td class="border px-8 py-4">165.0s</td>
  </tr>
  <tr class="hover:bg-gray-50">
    <td class="border px-8 py-4">VideoChatGPT</td>
    <td class="border px-8 py-4">Test</td>
    <td class="border px-8 py-4">videochatgpt</td>
    <td class="border px-8 py-4">Open-ended</td>
    <td class="border px-8 py-4">GPT_Eval</td>
    <td class="border px-8 py-4">ActivityNet-200</td>
    <td class="border px-8 py-4">120s</td>
  </tr>
    <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">VideoDetailCaptions</td>
        <td class="border px-8 py-4">Test</td>
        <td class="border px-8 py-4">video_dc499</td>
        <td class="border px-8 py-4">Open-ended</td>
        <td class="border px-8 py-4">GPT_Eval</td>
        <td class="border px-8 py-4">ActivityNet-200</td>
        <td class="border px-8 py-4">120s</td>
    </tr>
    <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">NextQA</td>
        <td class="border px-8 py-4">OE (Text / Validation), MC (Test)</td>
        <td class="border px-8 py-4">nextqa</td>
        <td class="border px-8 py-4">MCQ / Open-ended</td>
        <td class="border px-8 py-4">MC: Exact Match; OE: WUPS</td>
        <td class="border px-8 py-4">YFCC-100M</td>
        <td class="border px-8 py-4">44s</td>
    </tr>
    <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">CVRR-ES</td>
        <td class="border px-8 py-4">Default</td>
        <td class="border px-8 py-4">cvrr</td>
        <td class="border px-8 py-4">Open-ended</td>
        <td class="border px-8 py-4">GPT_Eval</td>
        <td class="border px-8 py-4">Internet; Public dataset</td>
        <td class="border px-8 py-4">22.3s</td>
    </tr>
        <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">Perception Test</td>
        <td class="border px-8 py-4">MC</td>
        <td class="border px-8 py-4">perceptiontest_val_mc</td>
        <td class="border px-8 py-4">MCQ</td>
        <td class="border px-8 py-4">Accuracy</td>
        <td class="border px-8 py-4">Internet</td>
        <td class="border px-8 py-4">23s</td>
    </tr>
    <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">TempCompass</td>
        <td class="border px-8 py-4">Caption Matching</td>
        <td class="border px-8 py-4">tempcompass_caption_matching</td>
        <td class="border px-8 py-4">Accuracy</td>
        <td class="border px-8 py-4">Internet</td>
        <td class="border px-8 py-4">30s</td>
    </tr>
    <tr class="hover:bg-gray-50">
        <td class="border px-8 py-4">Video-MME</td>
        <td class="border px-8 py-4">Test</td>
        <td class="border px-8 py-4">videomme</td>
        <td class="border px-8 py-4">MCQ</td>
        <td class="border px-8 py-4">Accuracy</td>
        <td class="border px-8 py-4">YouTube</td>
        <td class="border px-8 py-4">1017s</td>
    </tr>
    <!-- A,B,C,D QA	Accuracy	YouTube	1017.0s -->
</table>

### Alignment Check for Video Datasets

<!-- | Dataset             | Subset                                  | Model               | Original Reported | LMMs-Eval        | Official website for reference                                          |
| ------------------- | --------------------------------------- | ------------------- | ----------------- | ---------------- | ----------------------------------------------------------------------- |
| EgoSchema           | egoschema_mc_ppl                        | LLaVA-NeXT-Video-7B | -                 | Bo running on it | https://github.com/egoschema/EgoSchema/tree/main/benchmarking/mPLUG-Owl |
| CVRR-ES             | cvrr_multiple_actions_in_a_single_video | Video-ChatGPT       | 27.67             | 28.31            | https://github.com/mbzuai-oryx/CVRR-Evaluation-Suite/tree/main          |
| TempCompass         | tempcompass_caption_matching            | LLaVA-1.5-13B       | 59.50             | 59.35            | https://github.com/llyx97/TempCompass.git                               |
| VideoChatGPT        | videochatgpt_temporal                   | LLaVA-NeXT-Video-7B | 3.60              | Bo running on it | https://github.com/mbzuai-oryx/Video-ChatGPT.git                        |
| NextQA              | nextqa_oe_test                          | LLaVA-NeXT-Video-7B | 26.90             | Bo running on it |                                                                         |
| VATEX               | vatex_test                              | Gemini-1.5-Pro      |                   | Bo running on it |                                                                         |
| ActivityNetQA       | activitynetqa                           | LLaVA-NeXT-Video-7B | 53.5/3.2          | Bo running on it |                                                                         |
| VideoDetailCaptions | video_dc499                             | LLaVA-NeXT-Video-7B | 3.32              | Bo running on it |                                                                         |
| Perception Test     | perceptiontest_val_mc                   | LLaVA-NeXT-Video-7B | -                 | Bo running on it |                                                                         |
| YouCook2            | youcook2_val                            | LLaVA-NeXT-Video-7B | -                 | Bo running on it |                                                                         |
| Video-MME           | videomme                                | LLaVA-NeXT-Video-7B | -                 | Bo running on it |                                                                         | -->

## More Details and Feature Updates with `v0.2.0`

### **Improved Pipeline for Video Evaluations**

Here’s a breakdown of adding video datasets support, especially on how we implement the process from video caching, loading and feed to model to get response.

1.  **Download and Load Videos:** Video are being loaded during generation phase. We will host different video datasets on the huggingface and preprocess the video path for you in your huggingface cache folder. It is recommended to set `HF_HOME` before you use our evaluation suite so that you can manage the download place. After downloading the videos from huggingface hub, we unzip them into a local cache dir, where by default is `HF_HOME`.

    - The code specifically demonstrates the logic of how we handle video datasets in lmms-eval.

        ```python
            @retry(stop=(stop_after_attempt(5) | stop_after_delay(60)), wait=wait_fixed(2))
            def download(self, dataset_kwargs=None) -> None:
                # If the dataset is a video dataset,
                # Recursively search whether their is a zip and unzip it to the huggingface home
                if dataset_kwargs is not None and "video" in dataset_kwargs and dataset_kwargs["video"]:
                    hf_home = os.getenv("HF_HOME", "~/.cache/huggingface/")
                    cache_dir = dataset_kwargs["cache_dir"]
                    cache_dir = os.path.join(hf_home, cache_dir)
                    accelerator = Accelerator()
                    if accelerator.is_main_process:
                        cache_path = snapshot_download(repo_id=self.DATASET_PATH, repo_type="dataset")
                        zip_files = glob(os.path.join(cache_path, "**/*.zip"), recursive=True)

                        if not os.path.exists(cache_dir) and len(zip_files) > 0:
                            for zip_file in zip_files:
                                eval_logger.info(f"Unzipping {zip_file} to {cache_dir}")
                                shutil.unpack_archive(zip_file, cache_dir)

                    accelerator.wait_for_everyone()

                    if "builder_script" in dataset_kwargs:
                        builder_script = dataset_kwargs["builder_script"]
                        self.DATASET_PATH = os.path.join(cache_path, builder_script)
                        dataset_kwargs.pop("builder_script")

                    dataset_kwargs.pop("cache_dir")
                    dataset_kwargs.pop("video")
        ```

2.  **Format questions:** For each task, questions are formatted in the `<taskname>/utils.py` file. We parse each document from the Huggingface dataset, retrieve the questions, and formulate the input with any specified model-specific prompts.

    - The code specifically demonstrates the logic of how to implement question format.

        ```python
        # This is the place where you format your question
        def perceptiontest_doc_to_text(doc, model_specific_prompt_kwargs=None):
            if model_specific_prompt_kwargs is None:
                model_specific_prompt_kwargs = {}
            pre_prompt = ""
            post_prompt = ""
            if "pre_prompt" in model_specific_prompt_kwargs:
                pre_prompt = model_specific_prompt_kwargs["pre_prompt"]
            if "post_prompt" in model_specific_prompt_kwargs:
                post_prompt = model_specific_prompt_kwargs["post_prompt"]

            question = doc["question"]
            if "options" in doc:
                index = 0
                for op in doc["options"]:
                    if index == 0:
                        question += "\n" + "A. " + op
                    elif index == 1:
                        question += "\n" + "B. " + op
                    else:
                        question += "\n" + "C. " + op
                    index += 1
                post_prompt = "\nAnswer with the option's letter from the given choices directly."
            print("question\n")
            print(question)

            return f"{pre_prompt}{question}{post_prompt}"
        ```

3.  **Process results:** After the model generates results, each result is parsed and evaluated based on the corresponding evaluation metric. The choice of metric is based on the dataset’s official implementation on their official project website. We primarily use three types of metrics:

    **a. Accuracy:**
    For datasets with ground truth answers, we generate a score by comparing the model’s results with the ground truth. This metric is commonly used in multiple-choice QA tasks such as PerceptionTest-val and EgoSchema-subset.

    **b. GPT Evaluation:**
    For open-ended answers generated by the model, we apply OpenAI GPT API to evaluate the responses. This metric is often used in generation tasks like ActivityNetQA and VideoChatGPT.

    **c. Submission:**
    If the dataset does not provide ground truth answers and requires submission of inference results to a server for evaluation, we provide a submission file according to the dataset's official template. This metric is used in tasks like EgoSchema, Perception Test.

4.  **Aggregate results:**
    After evaluating each data instance, we aggregate the individual results to generate the overall evaluation metrics. Finally, we provide a summary table that consolidates all the evaluation results, similar to the one in Google’s Gemini report.
5.  **Grouped Tasks:**
    For tasks with multiple subsets, we group all subset tasks together. For example, the VideoChatGPT dataset includes three subsets: generic, temporal, and consistency. By running `--task videochatgpt`, all three subsets can be evaluated together, eliminating the need to specify each subset individually. We summarize all the grouped task names in Table 1. This pipeline ensures a thorough and standardized evaluation process for video LMMs, facilitating consistent and reliable performance assessment across various tasks and datasets. - This code denotes how we organize the group of tasks together.
    `yaml
    group: videochatgpt
    task:
    - videochatgpt_gen
    - videochatgpt_temporal
    - videochatgpt_consistency
        `

### **Supported Video Tasks**
  1. ActivityNet-QA
  2. EgoSchema
  3. YouCook2
  4. VATEX
  5. VATEX-ZH
  6. VideoChatGPT
  7. VideoDetailCaptions
  8. NextQA
  9. CVRR-ES
  10. Perception Test
  11. TempCompass
  12. Video-MME
  
### **Support Video Models**

We have supported more video models that can be used in LMMs-Eval. We now support evaluating video datasets using a one line command.

1. LLaVA-NeXT-Video
2. Video-LLaVA
3. LLaMA-VID
4. Video-ChatGPT
5. MPLUG-OWL

### **More Updates**

 1. For newly added tasks with different splits and metrics, we have adopted a naming rule in the format `{name}_{split}_{metric}`. For instance, `perceptiontest_val_mcppl` refers to the validation split of the PerceptionTest evaluation dataset, using multiple choice perplexity as the evaluation metric.
 2. We support `llava-next` series models with sglang. You can use the following command to launch evaluation with sglang support, that’s much more efficient when running on `llava-next-72b/110b`

    ```bash
    python3 -m lmms_eval \
    	--model=llava_sglang \
    	--model_args=pretrained=lmms-lab/llava-next-72b,tokenizer=lmms-lab/llavanext-qwen-tokenizer,conv_template=chatml-llava,tp_size=8,parallel=8 \
    	--tasks=mme \
    	--batch_size=1 \
    	--log_samples \
    	--log_samples_suffix=llava_qwen \
    	--output_path=./logs/ \
    	--verbosity=INFO
    ```

 3. We add a `force_download` mode to robustly handle the case that videos are not fully cached in local folder. You could add the args to task yaml file as the following commands. To support the evaluations that in machines that do not have access to internet, we add the `local_files_only` to support this feature.

    ```yaml
    dataset_path: lmms-lab/ActivityNetQA
    dataset_kwargs:
      token: True
      video: True
      force_download: False
      local_files_only: False
      cache_dir: activitynetqa
    model_specific_prompt_kwargs:
      default:
        pre_prompt: ""
        post_prompt: " Answer the question using a single word or phrase."

    metadata:
      version: 0.0
      gpt_eval_model_name: gpt-3.5-turbo-0613
    ```

 4. We found that sometimes the dataset downloading process will throw `Retry` or `HTTP Timedout` errors. To prevent this, we recommend disabling `hf_transfer` mechanism by setting this in your environment `export HF_HUB_ENABLE_HF_TRANSFER="0"`
 5. mmmu_group_img_val

    We aligned the results of LLaVA-NeXT 34B by combining images into one. That is, in multi-image testing, **all images are stitched together into one**, and tested as a single image. When tested separately (`mmmu_val`), the score was 46.7, and after combination (`mmmu_group_img_val`), the score was 50.1.

    Example:

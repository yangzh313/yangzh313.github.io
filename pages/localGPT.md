## Reference
	- [LocalGPT:构建本地问答知识库 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/666360711)
- ## Test
	- (base) PS E:\Resources\OneDrive\Resources\code\ai\localGPT> pip install llama-cpp-python
	  Looking in indexes: http://mirrors.aliyun.com/pypi/simple/
	    Downloading http://mirrors.aliyun.com/pypi/packages/af/a6/6b836876620823551650db19d217118b9ef0983a     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 10.8/10.8 MB 2.9 MB/s eta 0:00:00
	    Installing build dependencies ... done
	    Getting requirements to build wheel ... done
	    Installing backend dependencies ... done
	    Preparing metadata (pyproject.toml) ... done
	  Requirement already satisfied: typing-extensions>=4.5.0 in c:\users\administrator\anaconda3\lib\site-packages (from llama-cpp-python) (4.9.0)
	  Requirement already satisfied: numpy>=1.20.0 in c:\users\administrator\anaconda3\lib\site-packages (from llama-cpp-python) (1.24.3)
	  Collecting diskcache>=5.6.1 (from llama-cpp-python)
	    Downloading http://mirrors.aliyun.com/pypi/packages/3f/27/4570e78fc0bf5ea0ca45eb1de3818a23787af9b390c0b0a0033a1b8236f9/diskcache-5.6.3-py3-none-any.whl (45 kB)
	       ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 45.5/45.5 kB 1.1 MB/s eta 0:00:00
	  Requirement already satisfied: jinja2>=2.11.3 in c:\users\administrator\anaconda3\lib\site-packages (from llama-cpp-python) (3.1.2)
	  Requirement already satisfied: MarkupSafe>=2.0 in c:\users\administrator\anaconda3\lib\site-packages (from jinja2>=2.11.3->llama-cpp-python) (2.1.1)
	  Building wheels for collected packages: llama-cpp-python
	    Building wheel for llama-cpp-python (pyproject.toml) ... done
	    Created wheel for llama-cpp-python: filename=llama_cpp_python-0.2.39-cp311-cp311-win_amd64.whl size=2124319 sha256=ab26d07b10bf73c7053ff90e794f21301d401eeb0c492e25e080b5c228ed9b96
	    Stored in directory: c:\users\administrator\appdata\local\pip\cache\wheels\01\a7\78\a81573ea609dd267c2dd5692f2ab7f3dfa345a7301543e7f7d
	  Successfully built llama-cpp-python
	  Installing collected packages: diskcache, llama-cpp-python
	  Successfully installed diskcache-5.6.3 llama-cpp-python-0.2.39
	  (base) PS E:\Resources\OneDrive\Resources\code\ai\localGPT>
	  (base) PS E:\Resources\OneDrive\Resources\code\ai\localGPT> python run_localGPT.py --show_sources --device_type cpu
	  2024-02-07 14:59:59,605 - INFO - run_localGPT.py:244 - Running on: cpu
	  2024-02-07 14:59:59,605 - INFO - run_localGPT.py:245 - Display Source Documents set to: True
	  2024-02-07 14:59:59,606 - INFO - run_localGPT.py:246 - Use history set to: False
	  2024-02-07 15:00:00,042 - INFO - SentenceTransformer.py:66 - Load pretrained SentenceTransformer: hkunlp/instructor-large
	  load INSTRUCTOR_Transformer
	  max_seq_length  512
	  2024-02-07 15:00:02,705 - INFO - run_localGPT.py:132 - Loaded embeddings from hkunlp/instructor-large
	  2024-02-07 15:00:02,889 - INFO - run_localGPT.py:60 - Loading Model: TheBloke/Llama-2-7b-Chat-GGUF, on: cpu
	  2024-02-07 15:00:02,889 - INFO - run_localGPT.py:61 - This action can take a few minutes!
	  2024-02-07 15:00:02,889 - INFO - load_models.py:38 - Using Llamacpp for GGUF/GGML quantized models
	  llama_model_loader: loaded meta data with 19 key-value pairs and 291 tensors from ./models\models--TheBloke--Llama-2-7b-Chat-GGUF\snapshots\191239b3e26b2882fb562ffccdd1cf0f65402adb\llama-2-7b-chat.Q4_K_M.gguf (version GGUF V2)
	  llama_model_loader: Dumping metadata keys/values. Note: KV overrides do not apply in this output.
	  llama_model_loader: - kv   0:                       general.architecture str              = llama
	  llama_model_loader: - kv   1:                               general.name str              = LLaMA v2
	  llama_model_loader: - kv   2:                       llama.context_length u32              = 4096
	  llama_model_loader: - kv   3:                     llama.embedding_length u32              = 4096
	  llama_model_loader: - kv   4:                          llama.block_count u32              = 32
	  llama_model_loader: - kv   5:                  llama.feed_forward_length u32              = 11008
	  llama_model_loader: - kv   6:                 llama.rope.dimension_count u32              = 128
	  llama_model_loader: - kv   7:                 llama.attention.head_count u32              = 32
	  llama_model_loader: - kv   8:              llama.attention.head_count_kv u32              = 32
	  llama_model_loader: - kv   9:     llama.attention.layer_norm_rms_epsilon f32              = 0.000001
	  llama_model_loader: - kv  10:                          general.file_type u32              = 15
	  llama_model_loader: - kv  11:                       tokenizer.ggml.model str              = llama
	  llama_model_loader: - kv  12:                      tokenizer.ggml.tokens arr[str,32000]   = ["<unk>", "<s>", "</s>", "<0x00>", "<...
	  llama_model_loader: - kv  13:                      tokenizer.ggml.scores arr[f32,32000]   = [0.000000, 0.000000, 0.000000, 0.0000...
	  llama_model_loader: - kv  14:                  tokenizer.ggml.token_type arr[i32,32000]   = [2, 3, 3, 6, 6, 6, 6, 6, 6, 6, 6, 6, ...
	  llama_model_loader: - kv  15:                tokenizer.ggml.bos_token_id u32              = 1
	  llama_model_loader: - kv  16:                tokenizer.ggml.eos_token_id u32              = 2
	  llama_model_loader: - kv  17:            tokenizer.ggml.unknown_token_id u32              = 0
	  llama_model_loader: - kv  18:               general.quantization_version u32              = 2
	  llama_model_loader: - type  f32:   65 tensors
	  llama_model_loader: - type q4_K:  193 tensors
	  llama_model_loader: - type q6_K:   33 tensors
	  llm_load_vocab: special tokens definition check successful ( 259/32000 ).
	  llm_load_print_meta: format           = GGUF V2
	  llm_load_print_meta: arch             = llama
	  llm_load_print_meta: vocab type       = SPM
	  llm_load_print_meta: n_vocab          = 32000
	  llm_load_print_meta: n_merges         = 0
	  llm_load_print_meta: n_ctx_train      = 4096
	  llm_load_print_meta: n_embd           = 4096
	  llm_load_print_meta: n_head           = 32
	  llm_load_print_meta: n_head_kv        = 32
	  llm_load_print_meta: n_layer          = 32
	  llm_load_print_meta: n_rot            = 128
	  llm_load_print_meta: n_embd_head_k    = 128
	  llm_load_print_meta: n_embd_head_v    = 128
	  llm_load_print_meta: n_gqa            = 1
	  llm_load_print_meta: n_embd_k_gqa     = 4096
	  llm_load_print_meta: n_embd_v_gqa     = 4096
	  llm_load_print_meta: f_norm_eps       = 0.0e+00
	  llm_load_print_meta: f_norm_rms_eps   = 1.0e-06
	  llm_load_print_meta: f_clamp_kqv      = 0.0e+00
	  llm_load_print_meta: f_max_alibi_bias = 0.0e+00
	  llm_load_print_meta: n_ff             = 11008
	  llm_load_print_meta: n_expert         = 0
	  llm_load_print_meta: n_expert_used    = 0
	  llm_load_print_meta: rope scaling     = linear
	  llm_load_print_meta: freq_base_train  = 10000.0
	  llm_load_print_meta: freq_scale_train = 1
	  llm_load_print_meta: n_yarn_orig_ctx  = 4096
	  llm_load_print_meta: rope_finetuned   = unknown
	  llm_load_print_meta: model type       = 7B
	  llm_load_print_meta: model ftype      = Q4_K - Medium
	  llm_load_print_meta: model params     = 6.74 B
	  llm_load_print_meta: model size       = 3.80 GiB (4.84 BPW)
	  llm_load_print_meta: general.name     = LLaMA v2
	  llm_load_print_meta: BOS token        = 1 '<s>'
	  llm_load_print_meta: EOS token        = 2 '</s>'
	  llm_load_print_meta: UNK token        = 0 '<unk>'
	  llm_load_print_meta: LF token         = 13 '<0x0A>'
	  llm_load_tensors: ggml ctx size =    0.11 MiB
	  llm_load_tensors: offloading 0 repeating layers to GPU
	  llm_load_tensors: offloaded 0/33 layers to GPU
	  llm_load_tensors:        CPU buffer size =  3891.24 MiB
	  ..................................................................................................
	  llama_new_context_with_model: n_ctx      = 4096
	  llama_new_context_with_model: freq_base  = 10000.0
	  llama_new_context_with_model: freq_scale = 1
	  llama_kv_cache_init:        CPU KV buffer size =  2048.00 MiB
	  llama_new_context_with_model: KV self size  = 2048.00 MiB, K (f16): 1024.00 MiB, V (f16): 1024.00 MiB
	  llama_new_context_with_model:        CPU input buffer size   =    16.02 MiB
	  llama_new_context_with_model:        CPU compute buffer size =   308.00 MiB
	  llama_new_context_with_model: graph splits (measure): 1
	  AVX = 1 | AVX_VNNI = 0 | AVX2 = 1 | AVX512 = 0 | AVX512_VBMI = 0 | AVX512_VNNI = 0 | FMA = 1 | NEON = 0 | ARM_FMA = 0 | F16C = 1 | FP16_VA = 0 | WASM_SIMD = 0 | BLAS = 0 | SSE3 = 1 | SSSE3 = 0 | VSX = 0 |
	  Model metadata: {'general.name': 'LLaMA v2', 'general.architecture': 'llama', 'llama.context_length': '4096', 'llama.rope.dimension_count': '128', 'llama.embedding_length': '4096', 'llama.block_count': '32', 'llama.feed_forward_length': '11008', 'llama.attention.head_count': '32', 'tokenizer.ggml.eos_token_id': '2', 'general.file_type': '15', 'llama.attention.head_count_kv': '32', 'llama.attention.layer_norm_rms_epsilon': '0.000001', 'tokenizer.ggml.model': 'llama', 'general.quantization_version': '2', 'tokenizer.ggml.bos_token_id': '1', 'tokenizer.ggml.unknown_token_id': '0'}
- Enter a query: what is tunning
- llama_print_timings:        load time =  115957.43 ms
  llama_print_timings:      sample time =      38.82 ms /   208 runs   (    0.19 ms per token,  5357.51 tokens per second)
  llama_print_timings: prompt eval time =  150260.74 ms /   807 tokens (  186.20 ms per token,     5.37 tokens per second)
  llama_print_timings:        eval time =   52418.34 ms /   207 runs   (  253.23 ms per token,     3.95 tokens per second)
  llama_print_timings:       total time =  204136.42 ms /  1014 tokens
- > Question:
  what is tunning
- > Answer:
  Hello! I'm here to help you with any questions or tasks related to the context provided. In this case, the topic is "Instruction Tuning" in natural language processing.
  Based on the provided context, "tuning" refers to the process of fine-tuning pre-trained language models using instruction-following data to improve their performance on specific tasks. The context highlights the importance of using diverse and complex instructions in large-scale training data to enhance the ability of smaller models to trace the reasoning process of larger language models like GPT-4.
  To answer your question, "What is tunning?", I would explain that tunning is the process of fine-tuning pre-trained language models using instruction-following data to improve their performance on specific tasks. The term "tuning" is used in the context of natural language processing to refer to this process of fine-tuning pre-trained models to adapt them to specific tasks or domains.
  ----------------------------------SOURCE DOCUMENTS---------------------------
- > E:\Resources\OneDrive\Resources\code\ai\localGPT/SOURCE_DOCUMENTS\Orca_paper.pdf:
  2 Preliminaries
- 2.1
- Instruction Tuning
- Instruction tuning [22] is a technique that allows pre-trained language models to learn from input (natural language descriptions of the task) and response pairs, for example, {"instruction": correct sentence.", "input": fox jumped quickly"}. Instruction tuning has been applied to both language-only and multimodal tasks. For language-only tasks, instruction tuning has been shown to improve the zero-shot and few-shot performance of models such as FLAN [22] and InstructGPT [5] on various benchmarks. For multimodal tasks, instruction tuning has been used to generate synthetic instruction-following data for language-image tasks, such as image captioning [23] and visual question answering [24].
- "Arrange the words in the given sentence to form a grammatically
- "the quickly brown fox jumped", "output":
- "the brown
- > E:\Resources\OneDrive\Resources\code\ai\localGPT/SOURCE_DOCUMENTS\Orca_paper.pdf:
  "Arrange the words in the given sentence to form a grammatically
- "the quickly brown fox jumped", "output":
- "the brown
- A wide range of works in recent times, including Alpaca [7], Vicuna [9], WizardLM [8] and Koala [14], have adopted instruction-tuning to train smaller language models with outputs generated from large foundation models from the GPT family. As outlined in Section 1.1, a significant drawback with all these works has been both limited task diversity, query complexity and small-scale training data in addition to limited evaluation overstating the benefits of such approach.
- 2.2 Role of System Instructions
- Vanilla instruction-tuning (refer to Figure 4 for examples) often uses input, response pairs with short and terse responses. Such responses when used to train smaller models, as in existing works, give them limited ability to trace the reasoning process of the LFM. In constrast, system instructions10 in recent LFMs like GPT-4 can be used to provide guidance
- > E:\Resources\OneDrive\Resources\code\ai\localGPT/SOURCE_DOCUMENTS\Orca_paper.pdf:
  CoT NIV2 FLAN2021 T0 Dialog
- 150K 5M >28.9M 85.7M 22.5M
- 150K 440K 2.5M 2M 0
- Table 3: Construction of our training data with 5 million samples.
- > E:\Resources\OneDrive\Resources\code\ai\localGPT/SOURCE_DOCUMENTS\Orca_paper.pdf:
  3 Explanation Tuning
- To address the shortcomings of existing works, we tap into large-scale training data with diverse tasks augmented with complex instructions and rich signals. Specifically, our data contains human and augmented system instructions for a large collection of tasks sampled from FLAN-v2 (aka Flan 2022) [19]. Given the large size of the FLAN-v2 collection and varying number of examples for constituent datasets and tasks, we sample from a mixture of tasks from different categories (described in the next section) to create our training data.
- 3.1 Dataset Construction
  ----------------------------------SOURCE DOCUMENTS---------------------------
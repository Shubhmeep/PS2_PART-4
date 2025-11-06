# PS2_PART-4: Finetuning a TTS Model

This project involves fine-tuning a Text-to-Speech (TTS) model for Dutch, specifically using the SpeechT5 architecture. The process is split into two main notebooks: one for training the model and one for running inference (generating audio).


## ✅ `training.ipynb` – Fine-Tuning the SpeechT5 Model

This notebook handles the complete process of training the Dutch Text-to-Speech model.

### What it does

* **Data Loading:** Loads and preprocesses the Dutch speech dataset.
* **Data Collation:** Uses a custom data collator to correctly pad text and audio sequences. It also injects speaker embeddings during the batching process.
* **Model Configuration:** Sets up the SpeechT5 model architecture and configures all training hyperparameters (e.g., learning rate, batch size).
* **Fine-Tuning:** Runs the training loop, fine-tuning the model for multiple epochs on the target dataset.
* **Model Saving:** Saves all intermediate checkpoints and the final trained model directly to Google Drive. (This is necessary because the fine-tuned model is too large to store locally in a Colab environment).

### When to use it

Run this notebook **only if you want to retrain the model** from scratch or continue training from a previous checkpoint.



## ✅ `inference.ipynb` – Generate Audio Using the Fine-Tuned Model

This notebook provides a clean environment to use the fully trained model for speech synthesis.

### What it does

* **Load Model:** Loads the final, fine-tuned model weights directly from the saved location on Google Drive.
* **Reconstruct Pipeline:** Sets up the *exact same* preprocessing pipeline used during training. This includes the tokenizer (for text), the speaker embedding model, and the audio feature extractor.
* **Speech Generation:** Takes any arbitrary Dutch text input and uses the model to synthesize the corresponding speech.
* **Evaluation:** Outputs playable audio samples directly in the notebook, allowing for immediate evaluation of the model's performance.

### Speaker Similarity (SS) Evaluation

The `inference.ipynb` notebook also supports an optional evaluation step using a **Speaker Similarity (SS)** metric.  
For each sentence in the Dutch test split, we:
- Extract a speaker embedding from the original audio,
- Generate the same sentence with our TTS model,
- Extract a speaker embedding from the generated audio and compute **cosine similarity**.

We obtained a **mean SS score of about 0.9**, which shows that, even though the audio quality is still a bit rough, the model usually preserves the target **speaker identity** quite well.


## 🔊 Note on Inference and Replication

While the `inference.ipynb` notebook details the code used for speech synthesis, live replication of this process within the current environment is not feasible.

* **Reason:** The inference script depends on the final fine-tuned model checkpoint. This checkpoint file is exceptionally large and is stored externally on Google Drive.
* **Constraint:** Transferring and loading this large model file exceeds reasonable time and resource limits, precluding live execution and demonstration.

### Reviewing Pre-Generated Outputs

To demonstrate the model's performance, we have pre-generated several audio samples. You can review these outputs in two ways:

1.  **In the Notebook:** Audio samples are embedded directly within the `inference.ipynb` notebook for easy listening.
2.  **In the Repository:** All corresponding `.wav` files have been collected and uploaded to the `output_audio/` folder.

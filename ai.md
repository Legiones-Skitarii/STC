# Random AI Notes

1. runpod.io
2. vast.ai
    * Search -> On demand -> GPU
    * (Docker) Image select -> Pytorch 2.0.1 cuda11.7 devel
    * 100GB is good enough for most
    * Evening are costlier in evening (Cuz SF wakes up). MOrning time at 1am and 2am cheapest.
        * Unverified machines -> ENTIRE PLATFORM IS DECENTRALISED
            * Can get GPU for < 50$
3. lambda labs
4. Openaccess AI collective -> axolotl -> Best library for finetuning
Best for cost + availability -> Cheapest GPUs

Steps
1. Take 3090 from vast.ai -> Failed

(or)

2. Runpod.io -> Community cloud -> On demand / Interruptible -> A30
3. quickstart axolotl until finetune
4. axolotl -> Examples -> openllama 3B -> lightest option -> qlora.yml -> Can create own yml files
5. Model + Dataset -> `python scripts/finetune.py examples/openllama-3b/qlora.yml`
6. Adapter weights -> Weights which represent your finetuned model

Extra
* All datasets in JSON or JSONL
* Parquet format for compresssion

* Extra
    * Loss -> Distance from accuracy
    * learning_rate ->
    * Resources
        * 100 page ml book
        * The little book of deep learning - Francois Fleuret - https://fleuret.org/public/lbdl.pdf
* WandB -> Create acc -> Good for visualising data
* gradient descent / learning rate schedulers -> modify leraning rate on the fly

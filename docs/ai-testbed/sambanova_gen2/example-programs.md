# Example Programs

SambaNova provides examples of some well-known simple AI applications under the path: `/opt/sambaflow/apps/starters`, on all SambaNova compute nodes. Make a copy of this to your home directory:

```bash
cd ~/
mkdir apps
cp -r /opt/sambaflow/apps/starters apps/starters
```

## LeNet

Change directory

```bash
cd ~/apps/starters/lenet
```

### Common Arguments

Below are some of the common arguments used across most of the models in the example code.

| Argument               | Default   | Help                           |
|------------------------|-----------|--------------------------------|
| -b                     | 1         | Batch size for training        |
|                        |           |                                |
| -n,                    | 100       | Number of iterations to run    |
| --num-iterations       |           | the pef for                    |
|                        |           |                                |
| -e,                    | 1         | Number epochs for training     |
| --num-epochs           |           |                                |
|                        |           |                                |
| --log-path             | 'check    | Log path                       |
|                        | points'   |                                |
|                        |           |                                |
| --num-workers          | 0         | Number of workers              |
|                        |           |                                |
| --measure-train-       | None      | Measure training performance   |
| performance            |           |                                |
|                        |           |                                |

### LeNet Arguments

| Argument               | Default   | Help                           |
|------------------------|-----------|--------------------------------|
| --lr                   | 0.01      | Learning rate for training     |
|                        |           |                                |
| --momentum             | 0.0       | Momentum value for training    |
|                        |           |                                |
| --weight-decay         | 0.01      | Weight decay for training      |
|                        |           |                                |
| --data-path            | './data'  | Data path                      |
|                        |           |                                |
| --data-folder          | 'mnist_   | Folder containing mnist data   |
|                        | data'     |                                |
|                        |           |                                |

Establish the Environment

```bash
source /opt/sambaflow/apps/starters/lenet/venv/bin/activate
```

> **Note**:  If you receive an \"HTTP error\" message on any of the
following commands, run the command again. Such errors (e.g 503) are
commonly an intermittent failure to download a dataset.

Run these commands to compile and train the LeNet model:

```bash
srun python lenet.py compile -b=1 --pef-name="lenet" --output-folder="pef"
srun python lenet.py run --pef="pef/lenet/lenet.pef"
```

Alternatively to use Slurm sbatch, create submit-lenet-job.sh with the following
contents:

```bash
#!/bin/sh

python lenet.py compile -b=1 --pef-name="lenet" --output-folder="pef"
python lenet.py run --pef="pef/lenet/lenet.pef"
```

Then

```bash
sbatch --output=pef/lenet/output.log submit-lenet-job.sh
```

Squeue will give you the queue status.

```bash
squeue
# One may also...
watch squeue
```

One may see the run log using:

```bash
cat pef/lenet/output.log
```

## MNIST - Feed Forward Network

Establish the Environment

```bash
source /opt/sambaflow/apps/starters/ffn_mnist/venv/bin/activate
```

Change directory

```bash
cd ~/apps/starters/ffn_mnist/
```

Commands to run MNIST example:

```bash
srun python ffn_mnist.py  compile -b 1 --pef-name="ffn_mnist" --mac-v2
srun python ffn_mnist.py  run -b 1 -p out/ffn_mnist/ffn_mnist.pef
```

To run the same using Slurm sbatch, create and run the submit-ffn_mnist-job.sh with the following contents.

```bash
#!/bin/sh
python ffn_mnist.py  compile -b 1 --pef-name="ffn_mnist" --mac-v2
python ffn_mnist.py  run -b 1 -p out/ffn_mnist/ffn_mnist.pef
```

```bash
sbatch --output=pef/ffn_mnist/output.log submit-ffn_mnist-job.sh
```

## Logistic Regression

Establish the Environment

```bash
source /opt/sambaflow/apps/starters/logreg/venv/bin/activate
```

Change directory

```bash
cd ~/apps/starters/logreg
```

### Logistic Regression Arguments

This is not an exhaustive list of arguments.

Arguments

| Argument            | Default     | Help                         | Step     |
|---------------------|-------------|------------------------------|----------|
| --lr                | 0.001       | Learning rate for training   | Compile  |
|                     |             |                              |          |
| --momentum          | 0.0         | Momentum value for training  | Compile  |
|                     |             |                              |          |
| --weight-decay      | 1e-4        | Weight decay for training    | Compile  |
|                     |             |                              |          |
| --num-features      | 784         | Number features for training | Compile  |
|                     |             |                              |          |
| --num-classes       | 10          | Number classes for training  | Compile  |
|                     |             |                              |          |
| --weight-norm       | na          | Enable weight normalization  | Compile  |
|                     |             |                              |          |

Run these commands:

```bash
srun python logreg.py compile --pef-name="logreg" --output-folder="pef"
srun python logreg.py run --pef="pef/logreg/logreg.pef"
```

To use Slurm, create submit-logreg-job.sh with the following contents:

```bash
#!/bin/sh
python logreg.py compile --pef-name="logreg" --output-folder="pef"
python logreg.py run --pef="pef/logreg/logreg.pef"
```

Then

```bash
sbatch --output=pef/logreg/output.log submit-logreg-job.sh
```

The output, pef/logreg/output.log, will look something like this:

```text
2023-03-08 21:18:25.168190: I tensorflow/core/platform/cpu_feature_guard.cc:193] This TensorFlow binary is optimized with oneAPI Deep Neural Network Library (oneDNN) to use the following CPU instructions in performance-critical operations:  AVX2 FMA
To enable them in other operations, rebuild TensorFlow with the appropriate compiler flags.
2023-03-08 21:18:25.334389: W tensorflow/compiler/xla/stream_executor/platform/default/dso_loader.cc:64] Could not load dynamic library 'libcudart.so.11.0'; dlerror: libcudart.so.11.0: cannot open shared object file: No such file or directory
2023-03-08 21:18:25.334430: I tensorflow/compiler/xla/stream_executor/cuda/cudart_stub.cc:29] Ignore above cudart dlerror if you do not have a GPU set up on your machine.
2023-03-08 21:18:26.422458: W tensorflow/compiler/xla/stream_executor/platform/default/dso_loader.cc:64] Could not load dynamic library 'libnvinfer.so.7'; dlerror: libnvinfer.so.7: cannot open shared object file: No such file or directory
2023-03-08 21:18:26.422701: W tensorflow/compiler/xla/stream_executor/platform/default/dso_loader.cc:64] Could not load dynamic library 'libnvinfer_plugin.so.7'; dlerror: libnvinfer_plugin.so.7: cannot open shared object file: No such file or directory
2023-03-08 21:18:26.422709: W tensorflow/compiler/tf2tensorrt/utils/py_utils.cc:38] TF-TRT Warning: Cannot dlopen some TensorRT libraries. If you would like to use Nvidia GPU with TensorRT, please make sure the missing libraries mentioned above are installed properly.
[Info][SAMBA]# Placing log files in /home/wilsonb/apps/starters/logreg/pef/logreg/logreg.samba.log
[Info][MAC]# Placing log files in /home/wilsonb/apps/starters/logreg/pef/logreg/logreg.mac.log
...

Epoch [1/1], Step [10000/60000], Loss: 0.4642
Epoch [1/1], Step [20000/60000], Loss: 0.4090
Epoch [1/1], Step [30000/60000], Loss: 0.3863
Epoch [1/1], Step [40000/60000], Loss: 0.3703
Epoch [1/1], Step [50000/60000], Loss: 0.3633
Epoch [1/1], Step [60000/60000], Loss: 0.3553
Test Accuracy: 91.40  Loss: 0.3014
2023-03-08T21:19:08 : [INFO][LIB][2688517]: sn_create_session: PEF File: pef/logreg/logreg.pef
```
<!---
## UNet
The UNet application example is provided in the the path : `/opt/sambaflow/apps/image/segmentation/`. As any other application, we first compile and then train the model using *compile* and *run* arguments respectively. 
The scripts containing the compile and run commands for UNet model can be accessed at [unet.sh](./files/unet.sh "unet.sh") or at `/data/ANL/scripts/Unet2d.sh` on any compute node. 

Change directory and copy files.

```bash
mkdir -p ~/apps/image/unet
cd ~/apps/image/unet
```

Copy and paste the contents of
[unet.sh](./files/unet.sh "unet.sh")
to a file with the same name into the current directory using your favorite editor.

```bash
chmod +x unet.sh
```

Run these commands for training (compile + train):

```bash
./unet.sh compile <image size> <batch_size>
./unet.sh run <image size> <batch_size>
```
For a image size of 256x256 and batch size 256, the commands are provided as follows. 
```bash
./unet.sh compile 256 256
./unet.sh run 256 256
```

If we inspect the compile and run commands for the UNet application provided in the script, 

```bash
python ${UNET}/compile.py compile -b ${BS}  --num-classes 2 --num-flexible-classes -1 --in-channels=3 --init-features 32 --in-width=${2} --in-height=${2} --enable-conv-tiling --mac-v2  --compiler-configs-file ${UNET}/jsons/compiler_configs/unet_compiler_configs_no_inst.json  --mac-human-decision ${UNET}/jsons/hd_files/hd_unet_${HD}_depth2colb.json --enable-stoc-rounding  --num-tiles ${NUM_TILES} --pef-name="unet_train_${BS}_${2}_single_${NUM_TILES}" > compile_${BS}_${2}_single_${NUM_TILES}.log 2>&1
```

```bash
srun --nodelist $(hostname) python ${UNET}/hook.py  run --data-cache=${CACHE_DIR}  --num-workers=${NUM_WORKERS} --in-channels=3 --in-width=${2} --in-height=${2} --init-features 32 --batch-size=${BS} --epochs 10  --data-dir ${DS} --log-dir log_dir_unet_${2}_${BS}_single_${NUM_TILES} --pef=$(pwd)/out/unet_train_${BS}_${2}_single_${NUM_TILES}/unet_train_${BS}_${2}_single_${NUM_TILES}.pef > run_unet_${BS}_${2}_single_${NUM_TILES}.log 2>&1
```

we see that the application is compiled with `--num-tiles 4`, which means that the entire application fits on 4 tiles or half of a RDU. 
The scripts currently logs to a file **run_unet_256_256_single_4.log** and the performance data is located at the bottom of file. The pef generated from the compilation process is placed under `out/unet_train_${BS}_${2}_single_${NUM_TILES}` inside the current working directory.   
--->

## Gpt 1.5B 
The Gpt 1.5B application example is provided in the the path : `/opt/sambaflow/apps/nlp/transformers_on_rdu/`. 
The scripts containing the `compile` and `run` commands for Gpt1.5B model can be accessed at [Gpt1.5B_single.sh](./files/Gpt1.5B_single.sh "Gpt1.5B_single.sh") or at `/data/ANL/scripts/Gpt1.5B_single.sh` on any SN30 compute node. This script is compiled and run for only 1 instance and the model fits on 4 tiles or half of a RDU. 

Change directory and copy files.

```bash
mkdir -p ~/apps/nlp/Gpt1.5B_single
cd ~/apps/nlp/Gpt1.5B_single
```
Copy and paste the contents of
[Gpt1.5B_single.sh](./files/Gpt1.5B_single.sh "Gpt1.5B_single.sh")
to a file with the same name into the current directory using your favorite editor.

or copy the contents from `/data/ANL/scripts/Gpt1.5B_single.sh`. 

```bash
cp /data/ANL/scripts/Gpt1.5B_single.sh ~/apps/nlp/Gpt1.5B_single/
```
Run the script. 
```bash
chmod +x Gpt1.5B_single.sh
./Gpt1.5B_single.sh
```

You can inspect the `compile` and `run` commands in the script to learn that this model trains with a batch size of 16 for 1 instance over 4 tiles. The human decision file and the compiler config file helps to optimize the compute and memory resources specific to this Gpt 1.5B model run. 

```bash
python /opt/sambaflow/apps/nlp/transformers_on_rdu/transformers_hook.py compile --module_name gpt2_pretrain --task_name clm --max_seq_length 1024 -b 16 --output_dir=${OUTDIR}/hf_output --overwrite_output_dir --do_train  --per_device_train_batch_size 16 --cache ${OUTDIR}/cache/ --tokenizer_name gpt2 --model_name gpt2 --mac-v2 --non_split_head --mac-human-decision /opt/sambaflow/apps/nlp/transformers_on_rdu/human_decisions_gm/mac_v2_overrides/gpt2_48_enc_full_recompute_training_spatialmapping_tiling16_clmerge_gm_nonpardp_lnsd.json --compiler-configs-file /opt/sambaflow/apps/nlp/transformers_on_rdu/human_decisions_gm/compiler_configs/compiler_configs_gpt2_sc_recompute_spatialmapping_tiling16_clsmerge_withcls_nonpardp_norc_e2e.json --skip_broadcast_patch --config_name /opt/sambaflow/apps/nlp/transformers_on_rdu/customer_specific/mv/configs/gpt2_config_xl_50260.json --no_index_select_patch --weight_decay 0.1  --max_grad_norm_clip 1.0 --num-tiles 4 --pef-name=gpt15_single --output-folder=${OUTDIR}
```

```bash
python /opt/sambaflow/apps/nlp/transformers_on_rdu/transformers_hook.py run  -b 16  --module_name gpt2_pretrain --task_name clm --max_seq_length 1024  --overwrite_output_dir --do_train  --per_device_train_batch_size 16 --cache ${OUTDIR}/cache/  --tokenizer_name gpt2 --model_name gpt2 --non_split_head --skip_broadcast_patch --no_index_select_patch --output_dir=${OUTDIR}/hf_output --config_name /opt/sambaflow/apps/nlp/transformers_on_rdu/customer_specific/mv/configs/gpt2_config_xl_50260.json --max_grad_norm_clip 1.0 --skip_checkpoint --data_dir /data/ANL/ss1024 --logging_steps 1 --max_steps 900000 --learning_rate 0.00025 --steps_this_run 100 --pef=${OUTDIR}/gpt15_single/gpt15_single.pef >> ${OUTPUT_PATH} 2>&1
```
The `squeue` command shows that the application runs on 4 tiles as shown below. 
```bash
/XRDU_0/RDU_0/TILE_0   2.1  96.9    0.8    0.1    0.0      0.0 796481  vsastry python /opt/sambaflow/apps/nlp/transformers_on_rdu/
/XRDU_0/RDU_0/TILE_1   2.1  96.9    0.8    0.1    0.0      0.0 796481  vsastry python /opt/sambaflow/apps/nlp/transformers_on_rdu/
/XRDU_0/RDU_0/TILE_2   2.5  96.9    0.4    0.1    0.0      0.0 796481  vsastry python /opt/sambaflow/apps/nlp/transformers_on_rdu/
/XRDU_0/RDU_0/TILE_3   2.5  96.9    0.4    0.1    0.0      0.0 796481  vsastry python /opt/sambaflow/apps/nlp/transformers_on_rdu/
/XRDU_0/RDU_0/TILE_4 100.0   0.0    0.0    0.0    0.0      0.0
/XRDU_0/RDU_0/TILE_5 100.0   0.0    0.0    0.0    0.0      0.0
...

```
# ATP-BytePS

ATP-BytePS is the endhost training system developed for the paper in [NSDI21]((https://www.usenix.org/conference/nsdi21/presentation/lao)). This is based on [BytePS v0.2.4](https://github.com/bytedance/byteps/tree/v0.2.4) substituting PS-lite communication library to ATP communication library. 

## Worker Installation
### Build from source code

```
git clone -b p4ml_024 git@github.com:laochanlam/byteps.git
cd byteps
sudo -E BYTEPS_WITHOUT_MXNET=1 BYTEPS_WITHOUT_TENSORFLOW=1 python setup.py install
```
The dependencies can be referred to [BytePS v0.2.4](https://github.com/bytedance/byteps/tree/v0.2.4). 
## Quick Start

### launch workers
```
sudo -E CUDA_VISIBLE_DEVICES=0 DMLC_WORKER_ID=0 DMLC_NUM_WORKER=2 DMLC_INTERFACE=enp178s0f0 DMLC_ROLE=worker DMLC_NUM_SERVER=1 DMLC_PS_ROOT_URI=192.168.0.3 DMLC_PS_ROOT_PORT=6767 EVAL_TYPE=benchmark P4ML_APP=1 python byteps/launcher/launch.py python byteps/example/pytorch/benchmark_byteps.py --model vgg16 --num-iters 10
```
```
sudo -E CUDA_VISIBLE_DEVICES=0 DMLC_WORKER_ID=1 DMLC_NUM_WORKER=2 DMLC_INTERFACE=enp178s0f0 DMLC_ROLE=worker DMLC_NUM_SERVER=1 DMLC_PS_ROOT_URI=192.168.0.3 DMLC_PS_ROOT_PORT=6767 EVAL_TYPE=benchmark P4ML_APP=1 python byteps/launcher/launch.py python byteps/example/pytorch/benchmark_byteps.py --model vgg16 --num-iters 10
```

## Run Parameter Server 
#### Compile and Run Server (Terminal4)
```
$ cd $ATP_REPO/server/
```
```
$ make
```
```
# Usage: ./app [AppID]
sudo ./app 1
```

### Run Tofino Switch 

#### Compile P4 Program and Start the Tofino Model (Terminal1)
If you are using physical switch, compile the switch program then jump to Terminal 2 directly.
```
$ cd $SDE
```
```
$ $TOOLS/p4_build.sh ~/git/p4ml/p4src/p4ml.p4
```
```
# (Optional) for software Tofino behavior model
$ ./run_tofino_model.sh -p p4ml
```
#### Load Specified Switch Program (Terminal2)
```
$ cd $SDE
```
```
$ ./run_switchd.sh -p p4ml
```
#### Enable Ports and Install Entries (Terminal3)
```
$ $SDE/run_p4_tests.sh -t $ATP_REPO/ptf/ -p p4ml 
```
```
$ $TOOLS/run_pd_rpc.py -p p4ml $ATP_REPO/run_pd_rpc/setup.py 
```

# Publications

- [NSDI'21] "[ATP: In-network Aggregation for Multi-tenant Learning](https://www.usenix.org/conference/nsdi21/presentation/lao)". ChonLam Lao, Yanfang Le, Kshiteej Mahajan, Yixi Chen, Wenfei Wu, Aditya Akella, Michael Swift.

# Contact

Any questions? Please feel free to reach us at inatpcontact@gmail.com. You are more likely to receive a helpful response if your question is specific, self-contained and concise.

# Acknowledgment
This repository is modified based on [BytePS v0.2.4](https://github.com/bytedance/byteps/tree/v0.2.4). 

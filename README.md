
<div align="center">

## **ByteAI: Decentralised AI-Curated News** <!-- omit in toc -->

[Telegram](https://t.me/ByteAI_Portal) • [Website](https://byteaitoken.com) • [Whitepaper](https://byteai.gitbook.io/byteai-whitepaper/roadmap) 

---

### SN1 and Bittensor Socials <!-- omit in toc -->
[SN1 Discord](https://discord.gg/bittensor) •
[License](https://opensource.org/licenses/MIT) 

[BTT Discord](https://discord.gg/bittensor) • [Network](https://taostats.io/) • [Research](https://bittensor.com/whitepaper)

</div>

---

This is an open-source implementation of Bittensor Subnet 1's (SN1) asynchronous miners by ByteAI. For more information, please see SN1's [official repository](https://github.com/opentensor/prompting).

This repository also contains the **official codebase for Bittensor Subnet 1 (SN1) v1.0.0+, which was released on 22nd January 2024**. To learn more about the Bittensor project and the underlying mechanics, [read here.](https://docs.bittensor.com/).

# SN1 Pre-Staging Testnet 102 Streaming Experiments
This README is in the pre-staging branch of SN1 designed to facilitate the development of streaming-capable miners, and running streaming experiments. The README that is currently on `main` is temporarily renamed as `README_main.md` as a reference. 

As of March 25th, 2024, SN1 **only supports miners with streaming capabilities**. 

## Important Points

### 1. What is streaming? 
Streaming is when the miner sends back chunks of data to the validator to enable getting partial responses before timeout. The benefits are two fold:
1. getting rewards for unfinished, high quality responses, and
2. enabling a stream-based UI for responsive frontends. 

### 2. How mining is affected 
Stream miners need to implement a new `forward` method that enables async communications to the validator. The template for miners can be found in `docs/stream_miner_template.md`. 

```bash
# To run the miner
python <MINER_PATH>
    --netuid 102
    --subtensor.network test
    --wallet.name <your miner wallet> # Must be created using the bittensor-cli
    --wallet.hotkey <your validator hotkey> # Must be created using the bittensor-cli
    --logging.debug # Run in debug mode, alternatively --logging.trace for trace mode
    --axon.port #VERY IMPORTANT: set the port to be one of the open TCP ports on your machine
```
where `MINER_PATH` is either: 
1. neurons/miners/huggingface/miner.py
2. neurons/miners/openai/miner.py
 

### 3. Registration 
To do so: 
`btcli subnet register --netuid 102 --subtensor.network test --wallet.name <YOUR_WALLET_NAME> --wallet.hotkey <YOUR_HOTKEY_NAME>`

### 4. Mining checks 
Use the script in `scripts/client.py`. You can query your miner from a **separate** registered hotkey. This must be done because the same hotkey cannot ping itself. 

```bash
python scripts/client.py --uids YOUR_UIDS --wallet_name <YOUR_WALLET_NAME> --hotkey <YOUR_DIFFERENT_HOTKEY> --message "WHATEVER MESSAGE YOU WANT TO SEND"
```

An example is:
```bash
python scripts/client.py --wallet_name testnet --hotkey my_validator --message "Write me a 500 word essay about albert einstein" --uids 1 2
```

In order to query the networek, the hotkey you provide will need to have a vpermit. Until netuid 102 has more than 64 neurons registered, all neurons will have a vpermit by default. To check your neurons on netuid 102, run the following btcli command: 

```bash
btcli wallet overview --subtensor.network test --wallet.name <YOUR_WALLET_NAME> 
```

You can also wait to get queried by the validators, and pull the appropriate wandb data to checkout that your miners are being queried will all the data needed for analysis. Filtering by `netuid=102` is important here so that you see the active validators running on testnet, and previous test data. 

[Click here to be taken to the wandb validator page filtered by netuid 102.](https://wandb.ai/opentensor-dev/alpha-validators?nw=nwusermccrinbcsl) 

### 7. Installation
You will need to reinstall the repo to ensure that you do not see any errors. Please run:
```bash
python -m pip install -e . --force-reinstall
```

> Important: vLLM currently faces a [notable limitation](https://github.com/vllm-project/vllm/issues/3012) in designating a specific GPU for model execution via code. Consequently, to employ a particular CUDA device for your model's operations, it's necessary to manually adjust your environment variable `CUDA_VISIBLE_DEVICES`. For instance, setting `export CUDA_VISIBLE_DEVICES=1,2` will explicitly define the CUDA devices available for use.

# Real-time monitoring with wandb integration

Check out real-time public logging by looking at the project [here](https://wandb.ai/opentensor-dev/alpha-validators)

## License
This repository is licensed under the MIT License.
```text
# The MIT License (MIT)
# Copyright © 2024 Yuma Rao

# Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
# documentation files (the “Software”), to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software,
# and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all copies or substantial portions of
# the Software.

# THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO
# THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
```

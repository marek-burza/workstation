# Workstation Provisioning

## Introduction

This is a set of Ansible scripts to automate:

- Installation of Podman
- Restrictive `ufw` firewall configuration. Based on [DigitalOcean's UFW instructions](https://www.digitalocean.com/community/tutorials/how-to-setup-a-firewall-with-ufw-on-an-ubuntu-and-debian-cloud-server).
- Installation of NVIDIA GPU support. Based on instructions for installing the [drivers](https://docs.nvidia.com/datacenter/tesla/driver-installation-guide/index.html) and [container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt).
- Removal of insecure CUPS
- Disabling swap
- Kernel hardening (`sysctl`)

## Prerequisites

```shell
sudo apt-get -yq update
sudo apt-get -yq install ansible-core
install -m 0700 -d $HOME/.ansible/tmp
```

## Installation

```shell
sudo ansible-playbook -i "localhost," --connection=local playbook.yaml
```

## The `fake-interview-malware-research` Skill

Caution: Put it at user level so it runs from the home directory, never from inside the repository it's checking - that way the repository's `.claude/`, `.mcp.json` and `CLAUDE.md` never get loaded.

## Transcriptor

`roles/tools/files/transcriptor.py` transcribes with [Cohere Transcribe](https://huggingface.co/CohereLabs/cohere-transcribe-03-2026). The model is gated: accept its terms on the Hub and export `HF_TOKEN`.

- Cohere normalises every segment to unit variance, so a segment of room noise looks as loud to it as speech, and it answers with "Thank you.", "Thanks for watching!" or a looping sentence. Its token probabilities are as confident on those as on real speech, so they cannot filter them.
- The energy VAD still cuts the segments, because Silero VAD scores quiet or muffled remote voices near zero and would drop whole sentences of the other party.
- `SpeechGate` runs Silero over each decoded segment and discards the text when Silero never passes 0.2, or when its mean stays under 0.1 and the text is two words or fewer. Text without Latin letters is discarded too, since the language is fixed to English.
- Long speech ends at the first 160 ms pause after 15 s, with a hard cap at 30 s. Cohere drops the unfinished trailing phrase of a segment cut mid-sentence, so cutting at a pause loses less.
- Measured on 8 hours of recorded calls against Parakeet TDT 0.6B v3 and Qwen3-ASR 1.7B transcripts, with immediate word repeats collapsed: 11.7% / 11.4% WER against 14.6% / 13.4% for Whisper large-v3 (lower on each of the 11 recordings against both references), and 24 segments of text over no reference speech against 239 for Whisper, about 4x faster per segment.

## To Do

- KeePassXC in air-gapped and encrypted VM

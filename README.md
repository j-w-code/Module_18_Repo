# Pychain and Streamlit Web App

## Pychain and Streamlit Web App
This project covers the creation and use of a rudimentary python based blockchain and the streamlit front end interface for users. 

>"Bitcoin solves this"

## Technologies
This project uses Pandas, Streamlit, Datetime, Hashlib, and Dataclasses to create the blockchain, hash and timestamp transactions. 

[Pandas](https://github.com/pandas-dev/pandas)
[Streamlit](https://github.com/streamlit)
[Datetime](https://github.com/python/cpython/blob/main/Lib/datetime.py)
[Hashlib](https://github.com/python/cpython/blob/main/Lib/hashlib.py)
[Dataclass](https://github.com/python/cpython/blob/main/Lib/dataclasses.py)

### Installation Guide
In order to use this program please import and utilize the following libraries and dependencies: 

```python
import streamlit as st
from dataclasses import dataclass
from typing import Any, List
import datetime as datetime
import pandas as pd
import hashlib
```

## Usage 
the following blocks of code are fundamental in executing the program. 

```python
@dataclass
class RecordTrade:
    sender: str
    receiver: str
    amount: float
```
This creates the initial class RecordTrade with the initial variables assigned value types. This class will be called again. 

```python
@dataclass
class Block:
    record: RecordTrade
    creator_id: int
    prev_hash: str = "0"
    timestamp: str = datetime.datetime.utcnow().strftime("%H:%M:%S")
    nonce: int = 0
```
This class creates the titular "Block" for our blockchain. It will hold all the block data, note RecordTrade is set as value for "record" variable in this class. 

```python
def hash_block(self):
        sha = hashlib.sha256()

        record = str(self.record).encode()
        sha.update(record)

        creator_id = str(self.creator_id).encode()
        sha.update(creator_id)

        timestamp = str(self.timestamp).encode()
        sha.update(timestamp)

        prev_hash = str(self.prev_hash).encode()
        sha.update(prev_hash)

        nonce = str(self.nonce).encode()
        sha.update(nonce)

        return sha.hexdigest()
```
This function, nested inside of the class Block object is the hashing portion of our blockchain. It takes the various data variables and encodes them into a sha256 hash, updates the hash, and then outputs the hash number for our blockchain. This is essential for our chain to function. 

```python
@dataclass
class PyChain:
    chain: List[Block]
    difficulty: int = 4

    def proof_of_work(self, block):

        calculated_hash = block.hash_block()

        num_of_zeros = "0" * self.difficulty

        while not calculated_hash.startswith(num_of_zeros):

            block.nonce += 1

            calculated_hash = block.hash_block()

        print("Wining Hash", calculated_hash)
        return block
```
This portion of code sets up the Pychain class itself. It defines the proof-of-work funtionality and sets the difficulty of the block. There are more nested functions within the class.

```python
if st.button("Add Block"):
    prev_block = pychain.chain[-1]
    prev_block_hash = prev_block.hash_block()
    new_block = Block(
        record = RecordTrade(sender, receiver, amount),
        creator_id=42,
        prev_hash=prev_block_hash
    )

    pychain.add_block(new_block)
    st.snow()
```
This portion of code initializes a streamlit button for the user to add a new block to the existing blockchain. It starts the count from the previous block [-1] and adds each subsequent block. It then gives visual confirmation feedback with the snow code for interactive flair. 

```python
pychain_df = pd.DataFrame(pychain.chain).astype(str)
st.write(pychain_df)
```
This code writes the output of the pychain class into a dataframe that can be viewed and indexed. The streamlit code the takes the pychain and outputs it to be displayed visually. 



![<alt text>](https://i.postimg.cc/FKB6ckyp/Screen-Shot-2022-08-17-at-5-51-07-PM.png)

A screenshot showing the initial Streamlit pychain app interface

![<alt text>](https://i.postimg.cc/dtYx8Yyb/Screen-Shot-2022-08-17-at-5-51-18-PM.png)

A screenshot showing the blockchain with genesis block ("0") and subsequent additions. 

![<alt text>](https://i.postimg.cc/T2rk244C/Screen-Shot-2022-08-17-at-5-51-56-PM.png)

A screenshot showing the Streamlit drop down block inspector functionality. 

![<alt text>](https://i.postimg.cc/7hVM3pv4/Screen-Shot-2022-08-17-at-5-52-38-PM.png)

Block inspector results. 

## Contributors

Jeffrey J. Wiley Jr

## License

MIT
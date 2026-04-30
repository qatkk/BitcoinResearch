# eCash History and Building Blocks 
Here I'll be writing my notes on how the fedimint and eCash protocols works. 

## eCash and Blind Signatures
- Each note has:
    - A really large random number which is the reference number of the note ( to keep track of the created notes and make sure it's not already spent)
    - Signature of the note 
        
        Note, that at the time of signing the note the federation entities are not seeing what they're signing. 
        `Question` How do they keep track of the references if they don't see the reference number when signing?
- How does trasnactions happen inside the federation (itermint)?

    Alice sends notes to Bob, Bob checks if the notes are valid by sending it to the mint and asking for an equivalent note. If the mint creates the note, then all good and the transaction goes through.
- What about intramint ones? 

    A really general idea is creating another HTLC within the Federation ( this could be really wrong though)
- In the first version of eCash the receiver had to be online to receive money. Why ? 

    Basically the reciever would receive the referene number for a note and has to check it with the mint to make sure it's not already been spent. 
- How did they solve this? 

    Cut and choose. A probabilistic algorithm to make sure that the receiver is receiving a reference for a note not spent before. 
- Detailed description of cut and choose please! 
    <!-- Todo -->

    Overal idea: If you try to double spend a note, it becomes obvious that you've cheated. 

- How is it that the bank doesn't see what it's signing and yet the user can't cheat the note for a bigger value?

    Initial implementations dealt with this using different public keys for different denominations. 

- If we have fixed denomination, how can we pay arbitary values? 

    If we want to pay 15 e-cash but have a 16 e-cash note, what shall we do?
        You basically have to give the 16 e-cash note and get a change in exchage. 
### Blind Signature Schemes (kind of)
While analyzing the security of blind signature schemes the adversery model is one-more-forgery. 
The scenario would be, having given `n` signatures to an adversary, is there any way it can create a new valid signature on `n+1`th message? 

<!--
To do
#### Diffie-Hellmann 
Note this is a key exchange scheme, and not a signature. 

#### Schnor Blind Signatures

#### BLS Signatures -->

### Cashu Protocol 
The description for their protocol can be found [here](https://gist.github.com/callebtc/557e4cc15f9e43d7474c7cb3d31ee8ed). Cashu uses the blind version of Diffie-Hellman instead of a signature (the idea and the concepts are quite similar though)

## Questions 

- Maybe a stupid question, but why isn't the list of spent coins just public?

 
## Resources 
- [Presentation from Pete_Winn](https://www.youtube.com/watch?v=G4iclApJL0c)
- [Adam Gibson's presentation on Chaumian eCash in Bitcoin](https://www.youtube.com/watch?v=VwMzNE1D3so&t=64s)
- [Adam Gibson's notes from the presentation](https://gist.github.com/AdamISZ/28c3475734becf031c9924f0b50d1bed)
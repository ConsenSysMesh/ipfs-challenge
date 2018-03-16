
## Intro

**InterPlanetary File System (IPFS)** is a protocol and network designed to create a content-addressable, peer-to-peer method of storing and sharing hypermedia in a distributed file system. For a great introduction to IPFS concepts, please check out this [blog post](https://medium.com/@ConsenSys/an-introduction-to-ipfs-9bba4860abd0) which gives both a high-level overview as well as a deeper dive into the technology.

Contrary to location based identifiers for files in HTTP, the content addressing system used by IPFS stores and retrieves data using an ID created from a hash of the content itself. This is known as a content ID. The protocol behind the content IDs used in IPFS is called Multihash, which has be designed to be modular and easily upgradable. Each hash is self-describing in that it specifies the hash function and length of the hash in the first two bytes of the multihash itself. For instance, at the time of this writing, content IDs for files on IPFS are prefixed with the letters `Qm` which is the the result of the first two bytes in hex  `1220` encoded in base58. *12* denotes that this is the SHA256 hash function and *20* is the length of the hash in bytes — 32 bytes. You can find more on the multihash protocol [here](https://multiformats.io/multihash/).

Data stored in IPFS and named links gives the collection of IPFS objects the structure of a **Merkle DAG** , or [DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph) meaning Directed Acyclic Graph, and Merkle to signify that this is a cryptographically authenticated data structure that uses cryptographic hashes to address content.

## The Challenge

The challenge proposed here is to build a system which acts as an event listener built upon [js-ipfs](https://github.com/ipfs/js-ipfs), reading data blobs and forwarding them to an IPFS node while hashing and validating the associated multihashes and data.

In this system, messages will be received as a stream of files from a directory for simplicity's sake, but please do not assume that your code would only handle a finite sequence of events. In other words, we expect your program to handle an arbitrarily large events stream. You would not be able to keep all events in memory or any other storage. These text files can be found within the appropriately named files directory. Additionally, there is a bash script within the repository that can be used to generate new files using the command `sh generate_files.sh`. This can be useful for testing against a variety of randomized input text.

The goal is to create a series of IPFS Merkle DAG nodes through loading each file and sending them to a pipeline that will:

1. create the node,
2. compute its multihash,
3. upload the content,
4. and then verify that the uploaded content output matches the input when retrieved from IPFS.

In other words, with an example, following the above steps:

### Create the node and compute its multihash:

A message containing the string `Hello` should create the Merkle DAGNode multihash `QmPb9XxWLB7k1bKTn8nPLNmJsnbCuzwbkY1KP6n9a4BBNm`.

This should be validated by the IPFS node *put* method.

### Upload the content:

If there is no discrepancy, the message should then be uploaded to IPFS.

### Validation:

Finally the result of retrieving the content using the multihash should be compared to the input to ensure that it matches the initial input `Hello`.

---
### Output:

The validated messages should be printed to STDOUT as they are processed with a new line delineated (`\n`) list with the input string followed by a colon, space and multihash and if all messages are received and validated without errors, the event listener should print that all messages have been received and validated. The final output might look like:

```
Hello: QmZbj5ruYneZb8FuR9wnLqJCpCXMQudhSdWhdhp5U1oPWJ
World: QmQnEkXRJJ5AhLp1vWrqiQ7GbSX1iGTsfKEmuQFJGBbc58f
ALL MESSAGES RECEIVED
```

Please be advised that the IPFS add method wraps input data with some internal metadata: so multihashes from the Merkle DAGNode create and IPFS node put methods will differ from the output from the corresponding *add* method.

This is okay. It is only necessary to validate that the DAGNode *create* and IPFS node *put* multihashes match and that the input data, and output data match.

## Input Assumptions:

- All input will be valid -- there's no need to check for illegal characters in the directory of files.
- All input files will be named sequentially, but the content will be arbitrary.

## Helpful Links

* [The JavaScript implementation of IPFS](https://github.com/ipfs/js-ipfs)
* [Helpful examples of using js-ipfs in practice](https://github.com/ipfs/js-ipfs/tree/master/examples)
* [The DAGNode js-ipfs library for creating DAGNode objects](https://github.com/ipld/js-ipld-dag-pb)

## Additional Notes:

Please write automated tests and include them with your submission and finally, please also include a README that explains:

- An overview of your design decisions.
- How to run your code and tests, including how to install any dependencies
  your code may have.

## Deliverables:

Please bundle your code into a zip file and email it to your respective interviewer, along with any additional notes you might have.

## Assessment Criteria
We expect you to write code you would consider production-ready.
This means the code should be well-factored, without needless
duplication, follow good practices and be automatically verified.

What we will look at:
- Does your code fulfill the requirements described in the challenge section above?
- The design and implementation, how easy it is to understand and maintain your code.
- How was the software verified, if by automated tests or some other way.

### **Thank you!**

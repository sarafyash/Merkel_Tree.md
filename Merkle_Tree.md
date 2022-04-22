# Merkle Tee

## Introduction to Merkle Tree

Merkle tree also known as hash tree is a data structure used for data verification and synchronization. 
It is a tree data structure where each non-leaf node is a hash of it’s child nodes. All the leaf nodes are at the same depth and are as far left as possible. 
It maintains data integrity and uses hash functions for this purpose. 

A hash tree, also known as a Merkle tree, is a tree in which each leaf node is labelled with the cryptographic hash of a data block, and each non-leaf node is labelled with the cryptographic hash of its child nodes' labels.
Merkle tree is a fundamental part of blockchain technology. It is a mathematical **data structure** composed of hashes of different blocks of data, and which serves as a summary of all the transactions in a block. It also allows for efficient and secure verification of content in a large body of data. It also helps to verify the consistency and content of the data. Both Bitcoin and Ethereum use Merkle Trees structure. Merkle Tree is also known as ***Hash Tree***.

The concept of Merkle Tree is named after **Ralph Merkle**, who patented the idea in **1979**. Fundamentally, it is a data structure tree in which every leaf node labelled with the hash of a data block, and the non-leaf node labelled with the cryptographic hash of the labels of its child nodes. The leaf nodes are the lowest node in the tree.


## Architecture of Merkle Tree

A Merkle tree stores all the transactions in a block by producing a digital fingerprint of the entire set of transactions. It allows the user to verify whether a transaction can be included in a block or not.

Merkle trees are created by repeatedly calculating hashing pairs of nodes until there is only one hash left. This hash is called the Merkle Root, or the Root Hash. The Merkle Trees are constructed in a bottom-up approach.

Every leaf node is a hash of transactional data, and the non-leaf node is a hash of its previous hashes. Merkle trees are in a binary tree, so it requires an even number of leaf nodes. If there is an odd number of transactions, the last hash will be duplicated once to create an even number of leaf nodes.


![Merkle Tree](https://static.javatpoint.com/tutorial/blockchain/images/blockchain-merkle-tree.png)

The above example is the most common and simple form of a Merkle tree, i.e., **Binary Merkle Tree**. There are four transactions in a block: **TX1, TX2, TX3, and TX4**. Here you can see, there is a top hash which is the hash of the entire tree, known as the Root Hash, or the Merkle Root. Each of these is repeatedly hashed, and stored in each leaf node, resulting in Hash 0, 1, 2, and 3. Consecutive pairs of leaf nodes are then summarized in a parent node by hashing Hash0 and Hash1, resulting in Hash01, and separately hashing Hash2 and Hash3, resulting in Hash23. The two hashes (Hash01 and Hash23) are then hashed again to produce the Root Hash or the Merkle Root.

Merkle Root is stored in the **block header**. The block header is the part of the bitcoin block which gets hash in the process of mining. It contains the hash of the last block, a Nonce, and the Root Hash of all the transactions in the current block in a Merkle Tree. So having the Merkle root in block header makes the transaction tamper-proof. As this Root Hash includes the hashes of all the transactions within the block, these transactions may result in saving the disk space.

Merkle trees are used because, we want to limit the amount of data being sent over a network (like the Internet) as much as possible. So, instead of sending an entire file over the network, we just send a hash of the file to see if it matches.

![Breaking Data int bits of information](https://www.simplilearn.com/ice9/free_resources_article_thumb/Merkle_Tree_In_Blockchain_5.png)


## Algorithm 

1.	Computer A sends a hash of the file to computer B.
2.	Computer B checks that hash against the root of the Merkle tree.
3.	If there is no difference, we're done! Otherwise, go to step 4.
4.	If there is a difference in a single hash, computer B will request the roots of the two subtrees of that hash.
5.	Computer A creates the necessary hashes and sends them back to computer B.
6.	Repeat steps 4 and 5 until you've found the data blocks(s) that are inconsistent. It's possible to find more than one data block that is wrong because there might be more than one error in the data.

## Benefits of Merkle Tree in Blockchain 

Merkle trees provide four significant advantages - 
  1. Validate the data's integrity: It can be used to validate the data's integrity effectively.
  2. Takes little disk space: Compared to other data structures, the Merkle tree takes up very little disk space.
  3. Tiny information across networks: Merkle trees can be broken down into small pieces of data for verification.
  4. Efficient Verification: The data format is efficient, and verifying the data's integrity takes only a few moments.


## Code

```Java
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;

public class MerkleTree {

    public String createMerkleTree(ArrayList<String> txnLists) {
        ArrayList<String> merkleRoot = merkleTree(txnLists);
        return merkleRoot.get(0);
    }

    private ArrayList<String> merkleTree(ArrayList<String> hashList){
        //Return the Merkle Root
        if(hashList.size() == 1){
            return hashList;
        }
        ArrayList<String> parentHashList=new ArrayList<>();
        //Hash the leaf transaction pair to get parent transaction
        for(int i=0; i<hashList.size(); i+=2){
            String hashedString = getSHA(hashList.get(i).concat(hashList.get(i+1)));
            parentHashList.add(hashedString);
        }
        // If odd number of transactions , add the last transaction again
        if(hashList.size() % 2 == 1){
            String lastHash=hashList.get(hashList.size()-1);
            String hashedString = getSHA(lastHash.concat(lastHash));
            parentHashList.add(hashedString);
        }
        return merkleTree(parentHashList);
    }

    public static String getSHA(String input)
    {
        try {

            // Static getInstance method is called with hashing SHA
            MessageDigest md = MessageDigest.getInstance("SHA-256");

            // digest() method called
            // to calculate message digest of an input
            // and return array of byte
            byte[] messageDigest = md.digest(input.getBytes());

            // Convert byte array into signum representation
            BigInteger no = new BigInteger(1, messageDigest);

            // Convert message digest into hex value
            String hashtext = no.toString(16);

            while (hashtext.length() < 32) {
                hashtext = "0" + hashtext;
            }

            return hashtext;
        }

        // For specifying wrong message digest algorithms
        catch (NoSuchAlgorithmException e) {
            System.out.println("Exception thrown"
                    + " for incorrect algorithm: " + e);

            return null;
        }
    }

}
```

## References :

https://www.geeksforgeeks.org/introduction-to-merkle-tree/

https://www.simplilearn.com/tutorials/blockchain-tutorial/merkle-tree-in-blockchain

https://www.javatpoint.com/blockchain-merkle-tree

https://iq.opengenus.org/merkle-tree/

https://github.com/quux00/merkle-tree

https://medium.com/@vinayprabhu19/merkel-tree-in-java-b45093c8c6bd

https://rosettacode.org/wiki/SHA-256_Merkle_tree

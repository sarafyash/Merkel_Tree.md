# Merkle Tree

## Introduction to Merkle Tree
The Merkle tree, also known as the hash tree, is a data structure that is used for data verification and synchronisation.
It's a tree data structure in which each non-leaf node is a hash of its children. All of the leaf nodes are the same depth and as far to the left as possible.
It maintains data integrity by employing hash functions.

A hash tree, also known as a Merkle tree, is a tree in which each leaf node bears the cryptographic hash of a data block and each non-leaf node bears the cryptographic hash of the labels of its child nodes.
The Merkle tree is a key component of blockchain technology. It is a mathematical **data structure** made up of hashes of various blocks of data that serves as a summary of all transactions in a block. It also enables efficient and secure content verification across large datasets. It also aids in the validation of the data's consistency and content. Merkle Trees are used by Bitcoin and Ethereum. ***Hash Tree*** is another name for Merkle Tree.

Merkle Tree is named after **Ralph Merkle**, who patented the concept in **1979**. It is fundamentally a data structure tree in which each leaf node is labelled with the hash of a data block and the non-leaf node is labelled with the cryptographic hash of the labels of its child nodes. The leaf nodes are the tree's lowest nodes.


## Architecture of Merkle Tree

A Merkle tree records all transactions in a block by creating a digital fingerprint of the entire set of transactions. It enables the user to determine whether or not a transaction can be included in a block.

Merkle trees are formed by repeatedly hashing pairs of nodes until only one hash remains. This hash is known as the Merkle Root or Root Hash.The Merkle Trees are constructed in a bottom-up approach.

Each leaf node is a hash of transactional data, while the non-leaf node is a hash of its previous hashes. Merkle trees are binary trees, so they must have an even number of leaf nodes. If the number of transactions is odd, the last hash will be duplicated once to create an even number of leaf nodes.

![Merkle Tree](https://static.javatpoint.com/tutorial/blockchain/images/blockchain-merkle-tree.png)

The example above is the most common and basic type of Merkle tree, known as a **Binary Merkle Tree**. A block contains four transactions: **TX1**, **TX2**, **TX3**, and **TX4**. There is a top hash that is the hash of the entire tree, known as the Root Hash or the Merkle Root. Each of these is hashed repeatedly and stored in each leaf node, yielding Hash 0, 1, 2, and 3. Hashing Hash0 and Hash1, resulting in Hash01, and separately hashing Hash2 and Hash3, resulting in Hash23, are then used to summarise consecutive pairs of leaf nodes in a parent node. The two hashes (Hash01 and Hash23) are then hashed once more to produce the Root Hash or Merkle Root.
Merkle Root is stored in the **block header**. The block header is the part of the bitcoin block which gets hash in the process of mining. It contains the hash of the last block, a Nonce, and the Root Hash of all the transactions in the current block in a Merkle Tree. So having the Merkle root in block header makes the transaction tamper-proof. As this Root Hash includes the hashes of all the transactions within the block, these transactions may result in saving the disk space.

Merkle trees are used because we want to send as little data as possible over a network (such as the Internet). So, rather than sending the entire file over the network, we simply send a hash of the file to see if it matches.

![Breaking Data int bits of information](https://www.simplilearn.com/ice9/free_resources_article_thumb/Merkle_Tree_In_Blockchain_5.png)

## Verification

Merkle Trees are important because of their ability to efficiently verify data. Given any data from the list, we can determine whether it is valid or not in O(h) time. Furthermore, we do not require the entire list for verification.

Assume I received data C from a different server. Let's call this C'. We want to make sure C' hasn't been tampered with. We have a merkel tree containing all of the data in the list.
We only need the hashes in Merkel Tree. The diagram below shows how we can verify, C' in the absence of other data objects.

![Verification of Merkle tree ](https://miro.medium.com/max/1256/1*HwZtuEwJVDvEJio4OOCKpw.jpeg)

 1. Find the position of the C’ in the list. Probably by searching by id.
 2. Calculate the the hash of C’
 3. Calculate the value of the parent node by hashing the current node with its neighbor ( next if position is odd and previous if position in even) and set the parent     as the current node.
 4. Repeat step 3 until we find the root
 5. Compare the root with the previous root, if they match then C’
Contrast the new root with the old root. If the new root matches, the C' is still C and has not been tampered with.
In the case of a hash chain, it takes O(n) time to verify a data because we calculate n hashes in the worst case, whereas in the case of a Merkel Tree, the same data can be verified in O(logn) time because we only calculate logn hashes.

## Mathematical Representation

  ### Creation 
  
  As already mentioned before, Merkel Tree are created by taking two nodes from each layer and hashing them to create the parent node. by representing the tree in       matrix form we can mathematically write it as:
  
  
 ![Verification of Merkle tree ](https://miro.medium.com/max/1400/1*EA7FYersQE_oS6SLBuxlNQ.jpeg)
  
  This makes the root of the tree available at tree[0][0]
  
  ### Verification
  Verification is a bottom-up approach where we start from the data, find its hash and calculate the parent and continue this until we find the root. Mathematically,     we can express it as follows:
  
  ![Verification of Merkle tree ](https://miro.medium.com/max/1400/1*gGD-kH3a1_CoYeHyYLeodA.jpeg)
  
  
  
  
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

https://medium.com/coinmonks/implementing-merkle-tree-and-patricia-trie-b8badd6d9591#:~:text=To%20simply%20put%2C%20Merkel%20Trees,the%20two%20nodes%20below%20it.

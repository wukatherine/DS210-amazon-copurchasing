# Project Overview
**Goal: Friends-of-friends analysis.**

How often are a node’s connections connected to each other? Additionally, which two vertices who are friends have the most similar or most dissimilar sets of connections? 

**Dataset**: The dataset I used came from the Stanford SNAP website, and includes data from the Amazon website and product listings under the “Customers who purchased this item also bought” section under every product. Thus, it represents a directed graph with products as nodes and edges represent the item appearing under the other listing. 
View/download it here: https://snap.stanford.edu/data/amazon0601.html 

**Data Processing**:

I loaded my txt file into Rust by using it as the path in my main function. I had a function to create edges out of the graph using bufread for files. 

## Code Structure
**Module 1: graphs.rs**

This module contains the Graph struct and additional methods to create directed edges given an edges list. 

**Module 2: similarities.rs**

This module contains functions for different measures of similarity and functions that use the similarity measurements to analyze the graphs. 
Cluster coefficient tells how likely it is that friends of friends are friends with each other.
Jaccard similarity calculates the overlap between friends of nodes.
In the main code, the graphs.rs module is used to create the directed graph using the Amazon data. The functions in the similarities.rs module are then called onto the Graph to produce the final results. 

## Tests
Cargo test output: <img width="1133" height="209" alt="Screenshot 2026-07-22 at 11 27 21 AM" src="https://github.com/user-attachments/assets/0c96cee0-b03c-427b-a3f6-ba5194326a96" />

test_clustering checks the clustering coefficient, which calculates how likely that friends of friends are also friends. In the test, I made a sample graph, ran my clustering_coefficient function on the graph and node 0, and got the correct clustering coefficient, ⅔, as 2/3 of node 0’s friends were also linked - there are edges between (1, 2) and (2, 3), but not (1, 3). This is important because clustering_coefficient is used in full_similarity and no_similarity to get the nodes where each friend is friends with each other and where none are friends with each other, respectively. 
test_jaccard checks the calculation for jaccard similarity, which measures the overlap of friends for two nodes. I created a sample graph for this test, and then used my jaccard_similarity function to check the overlap between friends for nodes 0 in 1 in my sample graph. This is important because jaccard_similarity is used in greatest_jaccard and least_jaccard to get the friends that have the greatest overlap and the friends that have the least overlap in friends, respectively. 

## Results

<img width="1273" height="252" alt="Screenshot 2026-07-22 at 11 28 10 AM" src="https://github.com/user-attachments/assets/64a4f3c4-0a67-4f30-9c33-9339865eedc2" />

- Nodes 34158 and 25793 were friends that had the greatest Jaccard similarity out of all the pairs of linked nodes in the graph. Their similarity was 0.9, meaning 90% of their linked nodes overlapped. 
- Nodes 1 and 4 were friends which had the smallest overlap between friends, with a Jaccard similarity of 0.0, meaning that none of their other friends overlapped. It is possible that there are other nodes that also had this similarity too, but my function just returns the first instance of a similarity of 0.0, which was found quite early on. 
- I printed out the number of nodes with a clustering coefficient of 1.0, which meant that all their friends were friends, and there were 22,766 nodes for which this applied. This shows that for many nodes, their friends shared a similar network as them, which makes sense since Amazon probably promotes items that are similar to each other under each other. 
- I also printed out the number of nodes with a clustering coefficient of 0.0, which meant that none of their friends were friends, and there were 24,989 of them. 
- Another interesting takeaway is that 403,394 - 22,766 - 24,989 = 355,639, which means that a vast majority of the nodes had a clustering coefficient between 0 and 1, which definitely makes sense and shows that only some of a node’s friends are linked to each other. 

## Usage Instructions
- Build/run code using cargo run → make sure the ‘amazon0601.txt’ file is also located in the project folder
- With cargo run --release, the runtime is quick and should take no longer than a few minutes. 

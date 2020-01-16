# Tree Depth First Search
Using recursion (or use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be O(H), where ‘H’ is the maximum height of the tree.

## Example
### Count Paths for a Sum
Given a binary tree and a number ‘S’, find all paths in the tree such that the sum of all the node values of each path equals ‘S’. Please note that the paths can start or end at any node but all paths must follow direction from parent to child (top to bottom).
```
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


def count_paths(root, num):
    res = []
    stack = []
    stack.append([root])
    while stack:
        curr_path = stack.pop()
        for i in range(len(curr_path)):
            if path_sum(curr_path[i:len(curr_path)]) == num:
                res.append(curr_path[i:len(curr_path)])

        curr_node = curr_path[-1]
        if curr_node.left is not None:
            stack.append(curr_path + [curr_node.left])
        if curr_node.right is not None:
            stack.append(curr_path + [curr_node.right])
    print_res(res)
    return len(res)


def path_sum(path):
    return sum(map(lambda a: a.val, path))


def print_res(res):
    print('-'*10)
    for path in res:
        for idx, node in enumerate(path):
            print(node.val, end='')
            if idx != len(path) - 1:
                print('->', end='')
            else:
                print('\n')
    print('-'*10)


def main():
    root = TreeNode(12)
    root.left = TreeNode(7)
    root.right = TreeNode(1)
    root.left.left = TreeNode(4)
    root.right.left = TreeNode(10)
    root.right.right = TreeNode(5)
    print("Tree has paths: " + str(count_paths(root, 11)))

    root = TreeNode(1)
    root.left = TreeNode(7)
    root.right = TreeNode(9)
    root.left.left = TreeNode(6)
    root.left.right = TreeNode(5)
    root.right.left = TreeNode(2)
    root.right.right = TreeNode(3)
    print("Tree has paths: " + str(count_paths(root, 12)))


main()

```

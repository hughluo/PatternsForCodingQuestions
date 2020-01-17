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
### Tree Diameter
Given a binary tree, find the length of its diameter. The diameter of a tree is the number of nodes on the longest path between any two leaf nodes. The diameter of a tree may or may not pass through the root.

```
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


class TreeDiameter:

    def __init__(self):
        self.treeDiameter = 0

    def find_diameter(self, root):
        stack = []
        stack.append((root, False))
        res = []
        curr_paths = []
        while stack:
            curr_node, visited = stack[-1]  # we dont pop it now
            if not visited:
                stack[-1] = (curr_node, True)
                if curr_node.right is not None:
                    stack.append((curr_node.right, False))
                if curr_node.left is not None:
                    stack.append((curr_node.left, False))
            else:
                stack.pop()  # only pop the stack when the second time touch the node(when its subtree is visited)
                children_amount = self.get_chidren_amount(curr_node)
                if children_amount == 0:  # is leaf
                    curr_paths.append([curr_node.val])
                else:  # not leaf and all its leaf visited
                    # update res
                    curr_res = curr_paths[-1] + [curr_node.val]
                    if children_amount == 2:  # curr_node has two children
                        curr_res += curr_paths[-2][::-1]
                    if len(curr_res) > len(res):
                        res = curr_res

                    # make sure curr_paths only have the longer path of curr_paths
                    longer_path = curr_paths.pop()
                    if children_amount == 2:
                        another_path = curr_paths.pop()
                        if len(another_path) > len(longer_path):
                            longer_path = another_path
                    longer_path.append(curr_node.val)
                    curr_paths.append(longer_path)
        return res

    def get_chidren_amount(self, node):
        res = 0
        if node.left is not None:
            res += 1
        if node.right is not None:
            res += 1
        return res


def main():
    treeDiameter = TreeDiameter()
    root = TreeNode(1)
    root.left = TreeNode(2)
    root.right = TreeNode(3)
    root.left.left = TreeNode(4)
    root.right.left = TreeNode(5)
    root.right.right = TreeNode(6)
    print("Tree Diameter: " + str(treeDiameter.find_diameter(root)))
    root.left.left = None
    root.right.left.left = TreeNode(7)
    root.right.left.right = TreeNode(8)
    root.right.right.left = TreeNode(9)
    root.right.left.right.left = TreeNode(10)
    root.right.right.left.left = TreeNode(11)
    print("Tree Diameter: " + str(treeDiameter.find_diameter(root)))


main()

```

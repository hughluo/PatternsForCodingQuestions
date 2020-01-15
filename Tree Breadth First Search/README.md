# Tree Breadth First Search

Any problem involving the traversal of a tree in a level-by-level order can be efficiently solved using this approach. We will use a Queue to keep track of all the nodes of a level before we jump onto the next level. This also means that the space complexity of the algorithm will be O(W)O(W), where ‘W’ is the maximum number of nodes on any level.

## Examples
### Connect All Level Order Siblings
Given a binary tree, connect each node with its level order successor. The last node of each level should point to the first node of the next level.
```
from collections import deque


class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right, self.next = None, None, None

    # tree traversal using 'next' pointer
    def print_tree(self):
        print("Traversal using 'next' pointer: ", end='')
        current = self
        while current:
            print(str(current.val) + " ", end='')
            current = current.next


def connect_all_siblings(root):
    if not root:
        return
    queue = deque()
    queue.append(root)
    last_node = TreeNode('sentinel')

    while queue:
        level_node_count = len(queue)

        for _ in range(level_node_count):
            curr_node = queue.popleft()
            last_node.next = curr_node

            if curr_node.left is not None:
                queue.append(curr_node.left)
            if curr_node.right is not None:
                queue.append(curr_node.right)

            last_node = curr_node
    last_node.next = None


def main():
    root = TreeNode(12)
    root.left = TreeNode(7)
    root.right = TreeNode(1)
    root.left.left = TreeNode(9)
    root.right.left = TreeNode(10)
    root.right.right = TreeNode(5)
    connect_all_siblings(root)
    root.print_tree()


main()

```
### Right View of a Binary Tree
Given a binary tree, return an array containing nodes in its right view. The right view of a binary tree is the set of nodes visible when the tree is seen from the right side.
```
from collections import deque


class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None


def tree_right_view(root):
    result = []
    queue = deque()
    queue.append(root)
    while queue:
        level_node_count = len(queue)
        for i in range(level_node_count):
            curr_node = queue.popleft()

            if i == level_node_count - 1:
                result.append(curr_node)

            if curr_node.left is not None:
                queue.append(curr_node.left)
            if curr_node.right is not None:
                queue.append(curr_node.right)

    return result


def main():
    root = TreeNode(12)
    root.left = TreeNode(7)
    root.right = TreeNode(1)
    root.left.left = TreeNode(9)
    root.right.left = TreeNode(10)
    root.right.right = TreeNode(5)
    root.left.left.left = TreeNode(3)
    result = tree_right_view(root)
    print("Tree right view: ")
    for node in result:
        print(str(node.val) + " ", end='')


main()

```
### Tree Boundary
Given a binary tree, return an array containing all the boundary nodes of the tree in an anti-clockwise direction.


The boundary of a tree contains all nodes in the left view, all leaves, and all nodes in the right view. Please note that there should not be any duplicate nodes. For example, the root is only included in the left view; similarly, if a level has only one node we should include it in the left view.
```
from collections import deque


class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None


def find_tree_boundary(root):
    result = []
    left_view = []
    leafs = []
    right_view = []

    queue = deque()
    queue.append(root)
    while queue:
        level_node_count = len(queue)
        for i in range(level_node_count):
            curr_node = queue.popleft()
            if i == 0:
                left_view.append(curr_node)
            elif i == level_node_count - 1:  # `elif` instead of `if` is important to avoid duplicate
                right_view.append(curr_node)

            if curr_node.left is not None:
                queue.append(curr_node.left)
            if curr_node.right is not None:
                queue.append(curr_node.right)

    stack = []
    stack.append(root)
    while stack:
        curr_node = stack.pop()
        if curr_node.right is not None:
            stack.append(curr_node.right)
        if curr_node.left is not None:
            stack.append(curr_node.left)
        if curr_node.left is None and curr_node.right is None:
            leafs.append(curr_node)

    result = left_view + leafs[1:] + list(reversed(right_view))[1:]
    return result


def main():
    root = TreeNode(12)
    root.left = TreeNode(7)
    root.right = TreeNode(1)
    root.left.left = TreeNode(4)
    root.left.left.left = TreeNode(9)
    root.left.right = TreeNode(3)
    root.left.right.left = TreeNode(15)
    root.right.left = TreeNode(10)
    root.right.right = TreeNode(5)
    root.right.right.left = TreeNode(6)
    result = find_tree_boundary(root)
    print("Tree boundary: ", end='')
    for node in result:
        print(str(node.val) + " ", end='')

    root = TreeNode(12)
    root.right = TreeNode(1)
    root.right.left = TreeNode(10)
    root.right.right = TreeNode(5)
    result = find_tree_boundary(root)
    print("Tree boundary: ", end='')
    for node in result:
        print(str(node.val) + " ", end='')


main()

```

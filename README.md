# step-by-step-directions-from-a-binary-tree
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public String getDirections(TreeNode root, int startValue, int destValue) {
      // Find the lowest common ancestor (LCA) of startValue and destValue
        TreeNode lca = findLCA(root, startValue, destValue);

        // Generate paths from LCA to startValue and destValue
        StringBuilder startToLCA = new StringBuilder();
        StringBuilder lcaToDest = new StringBuilder();

        findPath(lca, startValue, new StringBuilder(), startToLCA);
        findPath(lca, destValue, new StringBuilder(), lcaToDest);

        // Convert all steps from start to LCA to 'U' (up)
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < startToLCA.length(); i++) {
            result.append('U');
        }

        // Append the steps from LCA to destination
        result.append(lcaToDest);

        return result.toString();
    }

    private TreeNode findLCA(TreeNode root, int p, int q) {
        if (root == null) {
            return null;
        }
        if (root.val == p || root.val == q) {
            return root;
        }

        TreeNode left = findLCA(root.left, p, q);
        TreeNode right = findLCA(root.right, p, q);

        if (left != null && right != null) {
            return root;
        }
        return left != null ? left : right;
    }

    private boolean findPath(TreeNode root, int value, StringBuilder path, StringBuilder result) {
        if (root == null) {
            return false;
        }
        if (root.val == value) {
            result.append(path);
            return true;
        }

        // Try left subtree
        path.append('L');
        if (findPath(root.left, value, path, result)) {
            return true;
        }
        path.deleteCharAt(path.length() - 1);

        // Try right subtree
        path.append('R');
        if (findPath(root.right, value, path, result)) {
            return true;
        }
        path.deleteCharAt(path.length() - 1);

        return false;  
    }
}

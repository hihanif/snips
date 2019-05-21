# Stacks

## 40. Evaluate Reverse Polish Notation
* avoid code duplication. little too much for board writing. but a good value addition to mention

```
interface Operator {
 int eval(int x, int y);
}
private static final Map<String, Operator> OPERATORS =
 new HashMap<String, Operator>() {{
 put("+", new Operator() {
 public int eval(int x, int y) { return x + y; }
 });
 put("-", new Operator() {
 public int eval(int x, int y) { return x - y; }
 });
 put("*", new Operator() {
 public int eval(int x, int y) { return x * y; }
 });
 put("/", new Operator() {
 public int eval(int x, int y) { return x / y; }
 });
 }};
public int evalRPN(String[] tokens) {
 Stack<Integer> stack = new Stack<>();
 for (String token : tokens) {
 if (OPERATORS.containsKey(token)) {
 int y = stack.pop();
 int x = stack.pop();
 stack.push(OPERATORS.get(token).eval(x, y));
 } else {
 stack.push(Integer.parseInt(token));
 }
 }
 return stack.pop();
}
```


##   Valid Parentheses
* how beautiful to push the inverse to ease the check. outstanding.
* missed the push itself in the first iteration
* missed to put the push in the second iteration

```java 
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new LinkedList<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch == '}' || ch == ']' || ch == ')') {
                if (stack.isEmpty()) return false;
                char popCh = stack.pop();
                if ((popCh == '{' && ch != '}') ||
                    (popCh == '(' && ch != ')') ||
                    (popCh == '[' && ch != ']')) 
                    return false;
            } else {
                stack.push(ch);
            }
        }
        return stack.isEmpty() ? true : false;
        
    }
}
```

```java
public boolean isValid(String s) {
	Stack<Character> stack = new Stack<Character>();
	for (char c : s.toCharArray()) {
		if (c == '(')
			stack.push(')');
		else if (c == '{')
			stack.push('}');
		else if (c == '[')
			stack.push(']');
		else if (stack.isEmpty() || stack.pop() != c)
			return false;
	}
	return stack.isEmpty();
}
```

* avoid code duplication with use of HashMap. a very good idea

```java
private static final Map<Character, Character> map =
 new HashMap<Character, Character>() {{
 put('(', ')');
 put('{', '}');
 put('[', ']');
 }};
public boolean isValid(String s) {
 Stack<Character> stack = new Stack<>();
 for (char c : s.toCharArray()) {
 if (map.containsKey(c)) {
 stack.push(c);
 } else if (stack.isEmpty() || map.get(stack.pop()) != c) {
 return false;
 }
 }
 return stack.isEmpty();
}
```

* Different from BFS, the nodes you visit earlier might not be the nodes which are closer to the root node. As a result, the first path you found in DFS might not be the shortest path.

## Clone Graph

* hanif - you did this - beautiful!
```java
class Solution {
    Map<Node, Node> map = new HashMap<>();
    
    public Node cloneGraph(Node node) {
        if (map.containsKey(node)) return map.get(node);
        if (node.neighbors == null) return new Node(node.val, null);
        
        List<Node> neighbors = new ArrayList<>();
        Node newNode = new Node(node.val, neighbors);
        map.put(node, newNode);
        
        for (Node n : node.neighbors) {
            neighbors.add(cloneGraph(n));
        }
        return newNode;
    }
}
```

## Inorder using stack

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();

    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode cur = root;

    while(cur!=null || !stack.empty()){
        while(cur!=null){
            stack.add(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        list.add(cur.val);
        cur = cur.right;
    }

    return list;
}
```

## 239. Decode String
* recursion looks better. be careful about isIndex(String, startIndex) api. the second arg has to match with your index

```java
public class Solution {
    public String decodeString(String s) {
        String res = "";
        Stack<Integer> countStack = new Stack<>();
        Stack<String> resStack = new Stack<>();
        int idx = 0;
        while (idx < s.length()) {
            if (Character.isDigit(s.charAt(idx))) {
                int count = 0;
                while (Character.isDigit(s.charAt(idx))) {
                    count = 10 * count + (s.charAt(idx) - '0');
                    idx++;
                }
                countStack.push(count);
            }
            else if (s.charAt(idx) == '[') {
                resStack.push(res);
                res = "";
                idx++;
            }
            else if (s.charAt(idx) == ']') {
                StringBuilder temp = new StringBuilder (resStack.pop());
                int repeatTimes = countStack.pop();
                for (int i = 0; i < repeatTimes; i++) {
                    temp.append(res);
                }
                res = temp.toString();
                idx++;
            }
            else {
                res += s.charAt(idx++);
            }
        }
        return res;
    }
}
```

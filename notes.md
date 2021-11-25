## Binary Search
***Mode***
'''
int binarySearch(int[] nums, int target){
    int left = 0;
    int right = nums.length-1;
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] == target){
            return mid;
        }else if(nums[mid] < target){
            left = mid + 1;
        }else if(nums[mid] > target){
            right = mid - 1;
        }
    }
    return -1;
}
'''
***Search left boundary of target***
There are duiplicate items in an array

'''
int leftBound(int[] nums, int target){
    int left = 0;
    int right = nums.length-1;
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] == target){
            right = mid-1;
        }else if(nums[mid] > target){
            right = mid-1;
        }else if(nums[mid] < target){
            left = mid + 1;
        }
    }
    if(left > nums.length||nums[left] != target){
        return -1;
    }
    return left;
}
'''

***Search right boundary of target***
There are duiplicate items in an array

'''
int rightBound(int[] nums, int target){
    int left = 0;
    int right = nums.length-1;
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] == target){
            left = mid + 1;
        }else if(nums[mid] > target){
            right = mid - 1;
        }else if(nums[mid] < target){
            left = mid + 1;
        }
    }
    if(right < 0||nums[right] != target){
        return -1;
    }
    return right;
}
'''

***Example***
x, f(x), target
1. f(x) is a mono function of x;
2. Try to find x that make f(x) == target

****Question**** 
Leetcode 87:Koko banana
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = Integer.MAX_VALUE;
        while(left <= right){
            int mid = left + (right - left)/2;
            int hours = getHour(mid,piles);
            if(hours == h){
                right = mid-1;
            }else if(hours > h){
                left = mid + 1;
            }else if(hours < h){
                right = mid - 1;
            }
        }
        return left;
        
    }
    
    private int getHour(int k, int[] piles){
        int res = 0;
        for(int i = 0; i < piles.length; i++){
            res += piles[i] / k;
            if(piles[i] % k != 0){
                res++;
            }
        }
        return res;
    }
}

Leetcode 1011:Capacity to ship packages within d days
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        int sum = 0;
        int max = 0;
        for(int i = 0; i < weights.length; i++){
            max = Math.max(max, weights[i]);
            sum += weights[i];
        }
        
        int left = max;
        int right = sum;
        
        while(left <= right){
            int mid = left + (right-left)/2;
            int value = getDays(mid, weights);
            if(value == days){
                right = mid - 1;
            }else if(value > days){
                left = mid + 1;
            }else if(value < days){
                right = mid - 1;
            }
        }
        return left;
        
    }
    
    private int getDays(int x, int[] weights){
        int days = 0;
        for(int i = 0; i < weights.length;){
            int cap = x;
                    while(i < weights.length){
                        if(cap < weights[i]){
                            break;
                        }else{
                                        cap -= weights[i];
                            
                        }

                i++;
        }
            days++;
        }

return days;
    }
}


Leetcode 410:Split Array Largest Sum
class Solution {
    public int splitArray(int[] nums, int m) {
        int max = 0;
        int sum = 0;
        for(int i = 0; i < nums.length; i++){
            max = Math.max(max, nums[i]);
            sum += nums[i];
        }
        
        int left = max;
        int right = sum;
        while(left <= right){
            int mid = left + (right-left)/2;
            int value = getSlice(nums,mid);
            if(value == m){
                right = mid - 1;
            }else if(value > m){
                left = mid + 1;
            }else if(value < m){
                right = mid - 1;
            }
        }
        return left;
        
    }
    
    int getSlice(int[] nums, int value){
        int slice = 0;
        for(int i = 0; i < nums.length;){
            int cap = value;
            while(i < nums.length){
                if(cap >= nums[i]){
                    cap -= nums[i];
                }else{
                    break;
                }
                i++;
            }
            slice++;
        }
        return slice;
        
    }
}

# Graph
## Dijkstra
***N-tree traverse***
'''
void levelTraverse(TreeNode root){
    if(root == null) return;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    int depth = 1;
    while(!queue.isEmpty()){
        for(int i = 0; i < queue.size(); i++){
            TreeNode cur = queue.poll();
            // printf();
            for(TreeNode child:cur.children){
                queue.offer(child);
            }
        }
        depth++;
    }
}
'''
***Graph BFS***
'''
void bfs(int node){
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(node);
    visited.add(node);
    int depth = 1;
    while(!queue.isEmpty()){
        for(int i = 0; i < queue.size(); i++){
            int cur = queue.poll();
            // Do what you want
            for(int neighbor:adj(cur)){
                if(!visited(neighbor)){
                    queue.offer(neighbor);
                    visited.add(neighbor);
                }
            }
        }
        depth++;
    }
}
'''

***Example***
Leetcode 743:Network delay time
O(ElogV)
class Solution {
    List<int[]>[] graph;
    public int networkDelayTime(int[][] times, int n, int k) {
        List<int[]>[] graph = new LinkedList[n+1];
        for(int i = 1; i <= n; i++){
            graph[i] = new LinkedList<>();
        }
        for(int[] time:times){
            graph[time[0]].add(new int[]{time[1], time[2]});
        }
        
        int[] dist = new int[n+1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        int[] parent = new int[n+1];
        dist[k] = 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->
            Integer.compare(a[1],b[1]));
        pq.offer(new int[]{k, 0});
        while(!pq.isEmpty()){
            int[] cur = pq.poll();
            int node = cur[0];
            int cost = cur[1];
            for(int[] nei:graph[node]){
                if(dist[nei[0]] > nei[1] + cost){
                    dist[nei[0]] = nei[1] + cost;
                    parent[nei[0]] = node;
                    pq.offer(new int[]{nei[0], dist[nei[0]]});
                }
            }
            
        }
        
        int max = 0;
        for(int i = 1; i <= n; i++){
            if(dist[i] == Integer.MAX_VALUE){
                return -1;
            }
            max = Math.max(max, dist[i]);
        }
        return max;

    }
    
    
}

***Leetcode 505*** Maze II
class Solution {
    int[][] dirs = {{0,1},{0,-1},{-1,0},{1,0}};
    int m;
    int n;
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        m = maze.length;
        n = maze[0].length;
        int[][] distance = new int[m][n];
        for(int[] row:distance){
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        distance[start[0]][start[1]] = 0;
        dijkstra(maze, start, distance);
        return distance[destination[0]][destination[1]] == Integer.MAX_VALUE?-1:distance[destination[0]][destination[1]];
        
    }
    
    public void dijkstra(int[][] maze, int[] start, int[][] distance){
        PriorityQueue<int[]> q = new PriorityQueue<>((a,b)->(a[2]-b[2]));
        q.offer(new int[]{start[0], start[1],0});
        while(!q.isEmpty()){
            int[] cur = q.poll();
            for(int[] dir:dirs){
                int x = cur[0] + dir[0], y = cur[1] + dir[1], count = 0;
                while(x >= 0 && y >= 0 && x < m && y < n && maze[x][y] == 0){
                    x += dir[0];
                    y += dir[1];
                    count++;
                }
                x -= dir[0];
                y -= dir[1];
                if(distance[cur[0]][cur[1]] + count < distance[x][y]){
                    distance[x][y] = distance[cur[0]][cur[1]] + count;
                    q.add(new int[]{x, y, distance[x][y]});
                }
            }
        }
    }
}

***787 Cheapest flights within k stops***
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for(int[] flight:flights){
            graph.computeIfAbsent(flight[0], value->new LinkedList<>()).add(new int[]{flight[1], flight[2]});
        }
        Map<Integer,Integer> visited = new HashMap<>();
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a, b) -> a[2] - b[2]); // dest, stops, cost
        pq.offer(new int[]{src, k+1,0});
        while(!pq.isEmpty()){
            int[] cur = pq.poll();
            int city = cur[0], stops = cur[1],cost = cur[2];
            if(stops < 0) continue;
            visited.put(city, stops);
            if(city == dst) return cost;
                List<int[]> nodes =graph.getOrDefault(city, new LinkedList<>());
                           for(int[] node:nodes){
                int neiCity = node[0];
                int neiCost = node[1];
                if(!visited.containsKey(neiCity) || stops > visited.get(neiCity)){
                    pq.offer(new int[]{neiCity, stops-1, cost + neiCost});
                }
        }
        }
        return -1;
   
}
}

## BFS
***LeetCode 37 Sudoko***
class Solution {
    public void solveSudoku(char[][] board) {
        solve(board, 0, 0);
    }
    
    public boolean solve(char[][] board, int row, int col){
        if(row == 9){ // note it is 9 not 8
            return true;
        }
        if(col == 9){
            return solve(board, row+1,0);
        }
        if(board[row][col] != '.'){
            return solve(board, row, col+1);
        }
        for(char c = '1'; c <='9'; c++){
            if(!isValid(board,row,col,c)){
                continue;
            }
            board[row][col] = c;
            if(solve(board, row, col+1)){
                return true;
            }
            board[row][col] = '.';
        }
        return false;
 
    }
    
    public boolean isValid(char[][] board, int i, int j, char c){
        for(int row = 0; row < 9; row++){
            if(board[row][j] == c) return false;
        }
        for(int col = 0; col < 9; col++){
            if(board[i][col] == c) return false;
        }
        for(int row = (i/3)*3; row <(i/3)*3 + 3; row++){
            for(int col = (j/3)*3; col < (j/3)*3 + 3; col++){
                if(board[row][col] == c) return false;
            }
        }
        return true;
    }
    
}

***22 Generate valid parenthesis***
class Solution {
    List<String> result;
    public List<String> generateParenthesis(int n) {
        result = new LinkedList<>();
        dfs(new StringBuilder(), n, n);
        return result;
        
    }
    
    private void dfs(StringBuilder sb, int leftCount, int rightCount){
        if(leftCount > rightCount){
            return;
        }
        if(leftCount < 0 || rightCount <0){
            return;
        }
        if(leftCount == 0 && leftCount == rightCount){
            result.add(sb.toString());
            return;
        }
        
        sb.append('(');
        dfs(sb, leftCount-1,rightCount);
        sb.deleteCharAt(sb.length()-1);
        
        sb.append(')');
        dfs(sb,leftCount, rightCount-1);
        sb.deleteCharAt(sb.length()-1);
        }
    
}

## Trie
***1804 Implement Trie***
class Trie {
    TrieNode root;

    public Trie() {
        root = new TrieNode();
        
    }
    
    public void insert(String word) {
        TrieNode node = root;
        for(int i = 0; i < word.length(); i++){
            char currentChar = word.charAt(i);
            if(!node.containsKey(currentChar)){
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
            node.count++;
        }
        node.setEnd();
        node.end++;
        
    }
    
    public int countWordsEqualTo(String word) {
        TrieNode current = root;
        int pos;
        for(char ch:word.toCharArray()){
            pos = ch-'a';
            if(current.links[pos] == null){
                return 0;
            }
            current = current.links[pos];
        }
        return current.end;
        
    }
    
    public int countWordsStartingWith(String prefix) {
        TrieNode current = root;
        int pos;
        for(char ch:prefix.toCharArray()){
            pos = ch -'a';
            if(current.links[pos] == null){
                return 0;
            }
            current = current.links[pos];
        }
        return current.count;
        
    }
    
    public void erase(String word) {
        TrieNode current = root;
        int pos;
        for(char ch:word.toCharArray()){
            pos = ch -'a';
            current = current.links[pos];
            current.count--;
        }
        current.end--;
        
    }
    
    class TrieNode{
        TrieNode[] links;
        int R = 26;
        boolean isEnd;
        int count;
        int end;
        public TrieNode(){
            links = new TrieNode[R];
        }
        
        public boolean containsKey(char ch){
            return links[ch-'a'] != null;
        }
        
        public TrieNode get(char ch){
            return links[ch-'a'];
        }
        
        public void put(char ch, TrieNode node){
            links[ch-'a'] = node;
        }
        
        
        
        public void setEnd(){
            isEnd = true;
        }
        
        public boolean isEnd(){
            return isEnd;
        }
    }
}


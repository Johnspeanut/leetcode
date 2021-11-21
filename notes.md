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







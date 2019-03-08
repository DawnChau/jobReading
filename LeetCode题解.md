### 001 求两个数的和为target的两个数的下标【HashMap】

- map里存这个数和这个数的下标
- 遍历一遍，如果存在和为target的数在map里，则返回两个索引

```java
public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int i = 0;i<nums.length;i++){
            if(map.containsKey(target-nums[i])){
                return new int[]{i,map.get(target-nums[i])};
            }
            map.put(nums[i],i);
        }
        return new int[]{0,0};
    }
```



### 002 把两个链表加起来，求和

- 由于链表是逆序的，直接从一开始就每个链表的值加起来
- 如果和为10，下一个节点需要加上1
- 如果加到结尾的话，就不生成下一个节点了

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode point = res;
        boolean end = false;
        while (l1 != null || l2 != null) {
            // 若是l1是空的了，只加l2的值
            if (l1 == null) {
                point.val += l2.val;
                if (l2.next == null)
                    end = true;
            } else if (l2 == null) {
                // 如果l2是空的了，只加l1的值
                point.val += l1.val;
                if (l1.next == null)
                    end = true;
            } else {
                point.val += l1.val + l2.val;
                if (l2.next == null && l1.next == null)
                    end = true;
            }

            if (point.val >= 10) {
                point.val -= 10;
                // 进位
                point.next = new ListNode(1);
            } else {
                // 没结束的话，还要为下一个继续生成节点
                point.next = end ? null : new ListNode(0);
            }
            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
            point = point.next;
        }
        return res;
    }
```

### 003 最长的没有重复的子串【HashSet】

- 思路，滑动窗口法
- HashSet存储，然后start，end窗口开始滑动
- 如果发现end指向的数存在，则窗口前移
- 如果发现end指向的数不存在，则加入到set里，窗口后移。并记录max

```java
public int lengthOfLongestSubstring(String s) {
        int start = 0;
        int end = 0;
        int res = 0;
        HashSet<Character> set = new HashSet<>();
        while (start < s.length() && end < s.length()) {
            if (set.contains(s.charAt(end))) {
                // 窗口前边后移
                set.remove(s.charAt(start++));
            } else {
                // 加入，并且后移
                set.add(s.charAt(end++));
                res = Math.max(res, end - start);
            }
        }
        return res;
    }
```

### 005 求最长回文子串

- 过一遍每一个字符，
- 对每一个字符左右扩展，记录max

```java
public static String longestPalindrome(String s) {
        // 长度为1也是回文的
        if (s.length() == 1 || s.length() == 0)
            return s;
        // 默认最长回文串的长度是1
        maxLength = 0;
        start = 0;
        for (int i = 0; i < s.length(); i++) {
            // 奇数
            solve(s, i, i);
            // 偶数
            solve(s, i, i + 1);
        }
        return s.substring(start, start + maxLength);

    }

    public static void solve(String s, int left, int right) {
        while (left >= 0 && right < s.length() && (s.charAt(left) == s.charAt(right))) {
            left--;
            right++;
        }
        // right-left-1 的解释，此刻left和right不应该被算进去
        if (maxLength < right - left - 1) {
            start = left + 1;
            maxLength = right - left - 1;
        }
    }
```

### 007 逆转整数

- 注意越界

```java
public int reverse(int x) {
        if (x == 0)
            return 0;
        boolean isNeg = x < 0 ? true : false;
        x = Math.abs(x);
        long res = 0;
        while (x > 0) {
            int val = x % 10;
            res = res * 10 + val;
            x /= 10;
        }
        if (res > Integer.MAX_VALUE || -1L * res < Integer.MIN_VALUE)
            return 0;
        return isNeg ? -1 * (int) res : (int) res;
    }
```

### 008 字符串转换为整数【BigDecimal】

- 首先先trim
- 判断并记录符号位
- Long 存不下，要用BigDecimal
- 越界判断

```java
public static int myAtoi(String str) {
        // 字符串消除前后空白
        str = str.trim();

        // 空字符串直接返回
        if (str.length() == 0)
            return 0;

        // 判断是否是符号位
        if (str.charAt(0) == '-') {
            str = str.substring(1); //去掉
            return strToInteger(str, false);
        } else if (str.charAt(0) == '+') {
            str = str.substring(1); // 去掉
            return strToInteger(str, true);
        } else if (Character.isDigit(str.charAt(0))) {
            // 如果不是符号位
            return strToInteger(str, true);
        }
        return 0;
    }

    public static int strToInteger(String str, boolean positive) {
        StringBuilder sb = new StringBuilder();
        // 必须使用BigDecimal才能接收
        BigDecimal res = new BigDecimal(0);

        for (int i = 0; i < str.length(); i++) {
            if (!Character.isDigit(str.charAt(i)))
                break;
            sb.append(str.charAt(i));
        }
        if (sb.length() != 0)
            res = new BigDecimal(sb.toString());
        if (!positive)
            res = res.multiply(new BigDecimal(-1));
        if (res.compareTo(new BigDecimal(Integer.MAX_VALUE)) > 0)
            return Integer.MAX_VALUE;
        if (res.compareTo(new BigDecimal(Integer.MIN_VALUE)) < 0)
            return Integer.MIN_VALUE;
        return res.intValue();
    }
```

### 011 最大蓄水面积

- 如果左边的比右边的矮，要右移左边的

```java
public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int max = Math.min(height[left], height[right]) * (right - left);
        while (left < right) {
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
            max = Math.max(max, Math.min(height[left], height[right]) * (right - left));
        }
        return max;
    }
```

### 013 罗马数字转为整数【HashMap】

- 如果左边的比右边的小，需要用右边的减掉两倍的左边的
- 原因是左边的曾经被加上过，正的-负的 = 两倍的正的

```java
public int romanToInt(String s) {
        if (s.length() == 0)
            return 0;
        Map<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        char pre = s.charAt(0);
        int res = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (map.get(pre) < map.get(c)) {
                res -= 2 * map.get(pre);
            }
            res += map.get(c);
            pre = c;
        }
        return res;
    }
```

### 014 最长公共前缀

- 递归退出条件：如果数组里只有一个字符串，那么最长公共前缀一定是该字符串
- 二分：求出左边的最长公共前缀和右边的最长公共前缀
- 最后相当于算两个字符串的最长公共前缀

```java
public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0)
            return "";
        int left = 0;
        int right = strs.length - 1;
        return solve(strs, left, right);
    }

    public String solve(String[] strings, int left, int right) {
        // 递归退出条件
        if (left == right)
            return strings[left];

        int mid = left + (right - left) / 2;
        String leftString = solve(strings, left, mid);
        String rightString = solve(strings, mid + 1, right);

        // 比较左右公共子串的公共子串
        int i = 0;
        for (; i < rightString.length(); i++) {
            if (i >= leftString.length() || leftString.charAt(i) != rightString.charAt(i))
                break;
        }
        return leftString.substring(0, i);
    }
```

### 015 求三个数的和等于0

- 小于3直接返回
- 排序
- 循环遍历，确定第一个
- 左右双指针确定第二个和第三个
- 注意去除重复

```java
public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums==null || nums.length<3)
            return res;

        Arrays.sort(nums);
        for(int i = 0;i<nums.length;i++){
            if(nums[i]>0)
                break;
            // 去除重复
            while (i>0 && i<nums.length && nums[i]==nums[i-1] ){
                i++;
            }
            int left = i+1;
            int right = nums.length-1;
            while(left<right){
                int sum = nums[i]+nums[left]+nums[right];
                if(sum==0){
                    List<Integer> inner = new ArrayList<>();
                    inner.add(nums[i]);
                    inner.add(nums[left++]);
                    inner.add(nums[right--]);
                    res.add(inner);
                    // 去除重复
                    while(left<right && nums[left]==nums[left-1]){
                        left++;
                    }
                    while (left<right && nums[right] == nums[right+1]){
                        right--;
                    }
                }else if(sum<0){
                    left++;
                    while(left<right && nums[left]==nums[left-1]){
                        left++;
                    }
                }else{
                    right--;
                    while (left<right && nums[right] == nums[right+1]){
                        right--;
                    }
                }
            }
        }
        return res;
```

### 017 电话按键的组合【HashMap】

- HashMap中存按键，字符串
- 递归退出的条件时按键次数 = 按键次数，此时把一种可能性加入到结果中
- 每一次按键选一个字符加入进去，进入更深层次递归
- 记得递归退出后删除刚刚加入的字符

```java
	private HashMap<Character,String> map = new HashMap<>();
    private List<String> list = new ArrayList<>();
    
    public List<String> letterCombinations(String digits) {
        if(digits==null || digits.length()==0)
            return list;
        map.put('2',"abc");
        map.put('3',"def");
        map.put('4',"ghi");
        map.put('5',"jkl");
        map.put('6',"mno");
        map.put('7',"pqrs");
        map.put('8',"tuv");
        map.put('9',"wxyz");
        // index 按键次数
        StringBuilder sb = new StringBuilder();
        solve(digits,0,sb);
        return list;
    }
    
    private void solve(String digits,int index,StringBuilder sb) {
        if(index==digits.length()){
            // 按键次数满了，可以加入
            list.add(sb.toString());
            return;
        }

        char c = digits.charAt(index);
        String s = map.get(c);
        for(int i = 0;i<s.length();i++){
            sb.append(s.charAt(i));
            solve(digits,index+1,sb);
            // 把刚刚加入的删除掉
            sb.deleteCharAt(sb.length()-1);
        }
    }
```

### 019 移除链表的倒数第n个节点

- 快慢指针，快的先走n步
- 然后同时走，确定倒数的位置
- 删除倒数第n个
- 注意删除头节点的情况

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
        // 快慢链表
        ListNode fast = head;
        ListNode slow = head;

        int count = 0;
        while(count<n){
            fast = fast.next;
            count++;
        }
        // 删除头
        if(fast==null){
            return head.next;
        }
        while(fast.next!=null){
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;
        return head;
    }
```

### 020 括号验证【Stack】

- 遇到左边括号，压入右边的括号
- 如果遇到右边括号，弹出的不和自己相等或者栈是空的，则返回false

```java
public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (Character c : s.toCharArray()) {
            switch (c) {
                case '{':
                    stack.push('}');
                    break;
                case '[':
                    stack.push(']');
                    break;
                case '(':
                    stack.push(')');
                    break;
                default:
                    if(stack.isEmpty() || stack.pop()!=c)
                        return false;
            }
        }
        return stack.isEmpty();
    }
```

### 021 合并两个有序的链表

- 先确定头
- 之后循环比较
- 一个为null之后把另一个剩下的添加上

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null)
            return l2;
        if(l2 == null)
            return l1;

        // 头 和 指针
        ListNode head = null;
        ListNode currentPoint = null;
        ListNode l1Point = null;
        ListNode l2Point = null;

    	// 确定头
        if(l1.val < l2.val){
            head = l1;
            l1Point = l1.next;
            l2Point = l2;
        }
        else{
            head = l2;
            l1Point = l1;
            l2Point = l2.next;
        }

        currentPoint = head;

        while(l1Point!=null && l2Point!=null){
            if(l1Point.val < l2Point.val){
                currentPoint.next = l1Point;
                currentPoint = currentPoint.next;
                l1Point = l1Point.next;
            }else{
                currentPoint.next = l2Point;
                currentPoint = currentPoint.next;
                l2Point = l2Point.next;
            }
        }

        if(l1Point == null){
            currentPoint.next = l2Point;
        }else if(l2Point == null){
            currentPoint.next = l1Point;
        }

        return head;
}
```

### 022 生成括号的全部种类

- 只能选择【0，n-1】，因为会手动加上一个，并且0是递归退出条件
- 核心思想是括号内和括号外的都是合理的括号

```java
public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        // 递归退出条件
        if(n == 0){
            res.add("");
            return res;
        }

        // 这里很关键，只能选择【0，n-1】，因为会手动加上一个，并且0是递归退出条件
        // 核心思想是括号内和括号外的都是合理的括号
        for (int i= 0;i<n;i++){
            List<String> first = generateParenthesis(i);
            List<String> second = generateParenthesis(n-i-1);
            for(String x:first){
                for (String y:second){
                    StringBuilder sb = new StringBuilder();
                    res.add(sb.append("(").append(x).append(")").append(y).toString());
                }
            }
        }
        return res;
    }
```

### 026 从有序数组里删除掉重复元素

- 使用不重复的覆盖掉前边的

```java
public int removeDuplicates(int[] nums) {
        if (nums.length == 0)
            return 0;
        int pre = nums[0];
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (pre != nums[i]) {
                pre = nums[i];
                nums[count++] = pre; //用不重复的数覆盖前几个
            }
        }
        return count;
    }
```

### 028 寻找第一次出现needle时候的索引

- 两重循环

```java
public int strStr(String haystack, String needle) {
        if (needle.length() == 0)
            return 0;
        for (int i = 0; i <= haystack.length() - needle.length(); i++) {
            for (int j = 0; j < needle.length(); j++) {
                if (haystack.charAt(i + j) != needle.charAt(j))
                    break;
                if (j == needle.length() - 1)
                    return i;
            }
        }
        return -1;
    }
```

### 029 不用除法实现两个数相除

- 为了防止越界，先把两个数变成long
- 特殊情况处理
- 相等及除数是1判断
- 除数每次乘以2，马上要超过的时候，用两者的差重新进行这个过程

```java
public static int divide(int dividend, int divisor) {
        // 记录正负
        boolean positive = true;

        // 扩展为long
        long longDivisor = divisor;
        long longDividend = dividend;

        // 特殊情况处理
        if (dividend == Integer.MIN_VALUE && divisor == -1)
            return Integer.MAX_VALUE;

        // 正负判断
        if ((longDividend < 0 && longDivisor > 0) || (longDividend > 0 && longDivisor < 0))
            positive = false;

        // 全部取正
        longDividend = Math.abs(longDividend);
        longDivisor = Math.abs(longDivisor);

        // 相等及除数是1的判断
        if (longDividend == longDivisor)
            return positive ? 1 : -1;
        if (longDivisor == 1)
            return positive ? dividend : -1 * dividend;

        int res = 0;
        int count = 0;
        long originDivisor = longDivisor;
        while (longDividend > longDivisor) {
            // 除数每次乘以2
            longDivisor <<= 1;
            // 记录幂数
            count++;
            if (longDivisor > longDividend) {
                // 超过了，回退一下
                longDivisor >>= 1;
                count--;
                // 剩余的差重新作为被除数
                longDividend = longDividend - longDivisor;
                // 除数恢复成最初的
                longDivisor = originDivisor;
                res += Math.pow(2, count);
                count = 0;
            } else if (longDividend == longDivisor) {
                // 恰好相等直接返回
                res += Math.pow(2, count);
                break;
            }
        }
        return positive ? res : -1 * res;
    }
```

### 033 在旋转过的数组里查找

- 核心思想二分
- 先判断哪个半区是正常顺序
- 每个半区分三种情况来讨论
- 注意判断半区要用else而不是else if

```java
public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;


        while (left <= right) {
            int mid = left + (right - left) / 2;


            if(left==right && nums[left]!=target)
                break;
            // 忽略下文中的等号判断
            if (nums[mid] == target)
                return mid;
            if (nums[right] == target)
                return right;
            if (nums[left] == target)
                return left;

            // 先判断哪个区是增序的
            if (nums[mid]>nums[left]) {
                // 左半区是增序的
                if (target > nums[mid]) {
                    // 属于右半区
                    left = mid + 1;
                } else {
                    if(target < nums[left]){
                        // 属于右半区
                        left = mid + 1;
                    }else{
                        // 属于左半区
                        right = mid - 1;
                    }
                }
            } else {
                // 这里要包括nums[mid]==nums[left] 和 nums[mid] < nums[left]
                // 右半区是增序的
                if (target < nums[mid]) {
                    // 属于左半区
                    right = mid - 1;
                } else {
                    if(target>nums[right]){
                        // 属于左半区
                        right = mid - 1;
                    }else{
                        // 属于右半区
                        left = mid + 1;
                    }
                }
            }
        }
        return -1;
    }
```

### 034 查找一个元素在数组里出现的首末位置

- 先用二分查找到这个数
- 之后将这个数往左右扩展，找出范围

```java
public int[] searchRange(int[] nums, int target) {
        if(nums == null || nums.length == 0)
            return new int[]{-1,-1};
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = left+(right-left)/2;
            if(target==nums[mid])
                return getRange(nums,mid);
            else if(target>nums[mid]){
                left = mid+1;
            }else{
                right = mid-1;
            }
        }
        return new int[]{-1,-1};
    }
    
    private int[] getRange(int[] nums, int mid) {
        int left = mid;
        int right = mid;
        int target = nums[mid];
        while(left>=0 && nums[left]==target)
            left--;
        while(right<nums.length && nums[right]==target)
            right++;
        return new int[]{++left,--right};
    }
```

### 036 验证数独的有效性【HashSet】

- 用Set来判断是否出现过
- 按照规则，横着，竖着，九宫格这样验证一遍即可

```java
public boolean isValidSudoku(char[][] board) {
        for(int i = 0;i<board.length;i++){
            Set<Character> set = new HashSet<>();
            for(int j = 0;j<board[0].length;j++){
                if(board[i][j]== '.')
                    continue;
                if(!set.add(board[i][j]))
                    return false;
            }
        }

        for(int i = 0;i<board[0].length;i++){
            Set<Character> set = new HashSet<>();
            for(int j = 0;j<board.length;j++){
                if(board[j][i]== '.')
                    continue;
                if(!set.add(board[j][i]))
                    return false;
            }
        }
        for(int row = 0;row<3;row++){
            for(int col = 0;col<3;col++){
                Set<Character> set = new HashSet<>();
                for(int i = 0;i<3;i++){
                    for(int j = 0;j<3;j++){
                        if(board[i+row*3][j+col*3]== '.')
                            continue;
                        if(!set.add(board[i+row*3][j+col*3]))
                            return false;
                    }
                }
            }
        }
        return true;
    }
```

### 038 数并说出来

- 遍历字符串，相等就加起来，然后最后把个数和该字符串一起形成一个新的字符串

```java
public String countAndSay(int n) {
        if(n == 1)
            return "1";
        StringBuilder sb = new StringBuilder("1");
        for(int j = 1;j<n;j++){
            char l = sb.charAt(0);
            int count = 0;
            StringBuilder newSb = new StringBuilder();
            for(int i = 0;i<sb.length();i++){
                char c = sb.charAt(i);
                if(c == l){
                    count++;
                }else{
                    newSb.append(""+count+l);
                    count = 1;
                    l = c;
                }
                if(i == sb.length()-1){
                    newSb.append(""+count+l);
                }
            }
            sb = newSb;
        }
        return sb.toString();
    }
```

### 046 全排列

- 第一步：确定该位置
- DFS该位置之后的元素
- 递归结束后再把该位置交换回来

```java
public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums.length == 0)
            return res;
        // 开始深度优先遍历
        dfs(res,nums,0);
        return res;
    }
    
    private void dfs(List<List<Integer>> res, int[] nums, int index) {
        // 深度遍历到底了
        if(index == nums.length){
            List<Integer> temp = new ArrayList<>();
            for(int i = 0;i<nums.length;i++){
                temp.add(nums[i]);
            }
            res.add(temp);
            return;
        }
        for (int i = index;i<nums.length;i++){
            // 确定该位置
            swap(nums,index,i);
            dfs(res,nums,index+1);
            // 再换回来
            swap(nums,index,i);
        }

    }

    private void swap(int[] nums, int index, int i) {
        int temp = nums[index];
        nums[index] = nums[i];
        nums[i] = temp;
    }
```

### 048 90度顺时针旋转矩阵

- 按对角线翻转
- 按照竖着的中轴线翻转

```java
public void rotate(int[][] matrix) {
        for (int i = 0; i < matrix.length; i++) {
            for (int j = i; j < matrix[0].length; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < (matrix[0].length+1)/2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][matrix.length-j-1];
                matrix[i][matrix.length-j-1] = temp;
            }
        }
    }
```

### 049 把由相同字符组成的单词分组【HashMap】

- 主要是把每个单词进行排序，这样顺序打乱也一样了
- HashMap 存放  排序后的单词，原单词的list

```java
public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> res = new ArrayList<>();
        HashMap<String,List<String>> map = new HashMap<>();
        for(int i = 0;i<strs.length;i++){
            char[] temp = strs[i].toCharArray();
            Arrays.sort(temp);
            String newStr = new String(temp);
            if(map.containsKey(newStr)){
                List<String> tempList = map.get(newStr);
                tempList.add(strs[i]);
                map.put(newStr,tempList);
            }else{
                List<String> tempList = new ArrayList<>();
                tempList.add(strs[i]);
                map.put(newStr,tempList);
            }
        }
        Iterator<String> it = map.keySet().iterator();
        while(it.hasNext()){
            res.add(map.get(it.next()));
        }
        return res;
    }
```

### 050 求一个数的幂

- 除二取余，余数为1就乘到ans上

```java
public double myPow(double x, int n) {
        if(n == 0)
            return 1.0;
        double ans = 1.0;
        for(int i = n;i!=0;i/=2){
            // 奇数时候过来乘一下子
            if(i%2!=0)
                ans*=x;
            x*=x;
        }
        if(n<0)
            return 1/ans;
        else
            return ans;
    }
```

### 053 最大和子数组

- 把每个数加起来，如果小于0，则和重置为0

```java
public int maxSubArray(int[] nums) {
        int sum = 0;
        int max = Integer.MIN_VALUE;
        for(int i = 0;i<nums.length;i++){
            sum = sum<0?0:sum;
            sum+=nums[i];
            max = Math.max(sum,max);
        }
        return max;
    }
```

### 055 看看能否跳到最后一个位置

- 从最后一个位置开始遍历
- 如果遇到比之间的差值大的，就代表着能够跳到这个位置上，将最后一个位置置为该位置
- 如果跳到头部还不行，则表示不能跳到最后一个位置

```java
public boolean canJump(int[] nums) {
        int lastIndex = nums.length-1;
        for(int i = lastIndex-1;i>=0;i--){
            if(nums[i]>=lastIndex-i)
                lastIndex = i;
            if(i == 0 && nums[i]<lastIndex-i)
                return false;
        }
        return true;
    }
```

### 062 独一无二的路径

- 公式 `dp[i][j] = dp[i-1][j]+dp[i][j-1]`

```java
public int uniquePaths(int m, int n) {
        int[][] dp = new int[m+1][n+1];
        for(int i = 1;i<=n;i++) {
            dp[1][i] = 1;
        }
        for(int i = 1;i<=m;i++) {
            dp[i][1] = 1;
        }
        for(int i = 2;i<=m;i++){
            for(int j = 2;j<=n;j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m][n];
    }
```

### 066 数组加一

- 如果最后一个数小于9，直接把最后一个数加一
- 如果最后一个数是9，则往前进位，直到进位到最前边的元素【此时全为9】

```java
public int[] plusOne(int[] digits) {
        if(digits[digits.length-1]<9){
            digits[digits.length-1]++;
            return digits;
        }else{
            int[] res = new int[digits.length+1];
            for(int i = digits.length-1;i>=0;i--){
                if(digits[i]==9){
                    digits[i]=0;
                }else{
                    digits[i]++;
                    return digits;
                }
            }
            res[0]=1;
            return res;
        }
    }
```

### 069 求开方

- 遍历一遍即可，注意用long

```java
public int mySqrt(int x) {
        for(long i = 1;;i++){
            long m = i*i;
            long n = (i+1)*(i+1);
            if(x>=m && x <n)
                return (int)i;
        }
    }
```

### 070 求爬楼梯的种数

- 斐波那契数列

```java
public int climbStairs(int n) {
        if( n ==1 || n ==2 )
            return n;
        int[] dp = new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3;i<n+1;i++){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
```

### 073 设置矩阵的横竖为0

- 用额外的数组存哪个位置是0
- 然后遍历原数行列组置为0

```java
public void setZeroes(int[][] matrix) {
        int[][] temp = new int[matrix.length][matrix[0].length];
        for(int i = 0;i<matrix.length;i++){
            for(int j = 0;j<matrix[0].length;j++){
                if(matrix[i][j]==0)
                    temp[i][j]=1;
            }
        }

        for(int i = 0;i<matrix.length;i++){
            for(int j = 0;j<matrix[0].length;j++){
                if(temp[i][j]==1) {
                    for (int m = 0; m < matrix[0].length; m++) {
                        matrix[i][m] = 0;
                    }
                    for (int m = 0; m < matrix.length; m++) {
                        matrix[m][j] = 0;
                    }
                }
            }
        }
    }
```

### 078 输出所有的子集

- 思路是先加单个的
- 然后在之前list的基础上再多加一个

```java
public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        res.add(new ArrayList<Integer>());

        for (int i = 0;i<nums.length;i++){
            List<Integer> temp = new ArrayList<>();
            temp.add(nums[i]);
            subsets(res,temp,nums,i);
        }
        return res;
    }
    private void subsets(List<List<Integer>> res, List<Integer> list, int[] nums,int index){
        res.add(list);
        for(int j = index +1 ;j<nums.length;j++){
            List<Integer> temp = new ArrayList<>(list);
            temp.add(nums[j]);
            subsets(res,temp,nums,j);
        }
    }
```

### 088 合并有序的数组

- 需要有一个数组暂存数组1
- 然后双指针将原来的数组填满

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
        // 暂存nums1
        int[] newArray = Arrays.copyOfRange(nums1,0,m);
        int i = 0;
        int j = 0;
        int index = 0;
        while(i<m && j<n){
            if(newArray[i]<nums2[j]){
                nums1[index++] = newArray[i++];
            }else{
                nums1[index++] = nums2[j++];
            }
        }
        if(i == m ){
            while(j<n){
                nums1[index++] = nums2[j++];
            }
        }else{
            while(i<m){
                nums1[index++] = newArray[i++];
            }
        }
    }
```

### 094 二叉树中序遍历（非递归）【Stack】

- 将当前节点和当前节点压入栈
- 当前节点= 左孩子
- 出栈
- 当前节点 = 右孩子

```java
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        if(root==null)
            return res;
        TreeNode cur = root;
        while(cur!=null || !stack.isEmpty()){
            while(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            res.add(cur.val);
            cur = cur.right;
        }
        return res;
    }
```












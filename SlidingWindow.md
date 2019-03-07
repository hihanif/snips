public class Solution {
	public String minWindow(String s, String t) {
		int[] map = new int[128];

		for (char c : t.toCharArray())
			map[c]++;

		int count = t.length(), begin = 0, end = 0, d = Integer.MAX_VALUE, head = 0;

		while (end < s.length()) {
			if (map[s.charAt(end)] > 0){
				count--;
			}
			map[s.charAt(end)]--;
			end++;
			while (count == 0) {
				if (end - begin < d) {
					d = end - begin;
					head = begin;
				}
				if (map[s.charAt(begin)] == 0)
					count++;
				map[s.charAt(begin)]++;
				begin++;
			}
		}

		return d == Integer.MAX_VALUE ? "" : s.substring(head, head + d);

	}
	
	public static void main(String[] args) {
		String S = "AOBECOEBANC", T = "ABC";
		System.out.println(new Solution().minWindow(S, T));
	}
}
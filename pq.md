//  Median of Two Sorted Arrays

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((o1, o2) -> (o2 - o1));
        
        for (int i = 0 ; i < nums1.length || i < nums2.length; i++) {
            if (i < nums1.length) minHeap.add(nums1[i]);
            if (i < nums2.length) minHeap.add(nums2[i]);
            
            if (minHeap.size() - maxHeap.size() > 1) {
                maxHeap.add(minHeap.poll());
            }
        }
        if (minHeap.size() == maxHeap.size()) {
            return ((double) (minHeap.peek() + maxHeap.peek())) / 2;
        }
        return (minHeap.size() > maxHeap.size()) ? (double)minHeap.peek() : (double)maxHeap.peek();
    }


// beautiful coding.

 public Interval[] intervalIntersection(Interval[] A, Interval[] B) {
        int minend = 0, maxstart = 0;
        List<Interval> res = new ArrayList<>();
        int i = 0, j = 0;
        while(i<A.length && j < B.length){
            Interval a = A[i];
            Interval b = B[j];
            minend = Math.min(a.end,b.end);
            maxstart = Math.max(a.start,b.start);
            if(minend >= maxstart){
                res.add(new Interval(maxstart,minend));
            }
            if(a.end == minend)i++;
            if(b.end == minend)j++;
        }
        return res.toArray(new Interval[res.size()]);
    }
===

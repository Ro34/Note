# 141
现在考虑一个环形链表，把慢指针和快指针想象成两个在环形赛道上跑步的运动员（分别称之为慢跑者与快跑者）。而快跑者最终一定会追上慢跑者。这是为什么呢？考虑下面这种情况（记作情况 A） - 假如快跑者只落后慢跑者一步，在下一次迭代中，它们就会分别跑了一步或两步并相遇。
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null||head.next ==null){
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;

        while(slow != fast){
            if(fast == null||fast.next ==null){
                return false;
            }
            fast = fast.next.next;
            slow = slow.next;
        }
        return true;
    }
}
```
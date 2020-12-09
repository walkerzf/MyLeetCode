## Design Twitter

> Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:
>
> postTweet(userId, tweetId): Compose a new tweet.
> getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
> follow(followerId, followeeId): Follower follows a followee.
> unfollow(followerId, followeeId): Follower unfollows a followee.
> Example:
>
> Twitter twitter = new Twitter();
>
> // User 1 posts a new tweet (id = 5).
> twitter.postTweet(1, 5);
>
> // User 1's news feed should return a list with 1 tweet id -> [5].
> twitter.getNewsFeed(1);
>
> // User 1 follows user 2.
> twitter.follow(1, 2);
>
> // User 2 posts a new tweet (id = 6).
> twitter.postTweet(2, 6);
>
> // User 1's news feed should return a list with 2 tweet ids -> [6, 5].
> // Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
> twitter.getNewsFeed(1);
>
> // User 1 unfollows user 2.
> twitter.unfollow(1, 2);
>
> // User 1's news feed should return a list with 1 tweet id -> [5],
> // since user 1 is no longer following user 2.
> twitter.getNewsFeed(1);

* 存在自己关注自己的情况
* 关注同样的人
* 取到每个对应的，必须先判断是不是为空，要是为空就会有```NullPointerException``

## Solution Map +PriorityQueue +set

```java
//time 55ms 45.5MB
import java.lang.reflect.Array;
import java.util.*;

class Twitter {
    Map<Integer, List<t>> userToTweetId;
    Map<Integer, Set<Integer>> followerToFollowee;
    int timeStamp =1;
    class t{
        int id;
        int time;
        public t(int id){
            this.id=id;
            this.time=timeStamp++;
        }
    }
    /**
     * Initialize your data structure here.
     */
    public Twitter() {
        userToTweetId = new HashMap<>();
        followerToFollowee = new HashMap<>();
    }

    /**
     * Compose a new tweet.
     */
    public void postTweet(int userId, int tweetId) {
        t  a=new t(tweetId);
        if (!userToTweetId.containsKey(userId)) {
            List<t> tmp = new LinkedList<>();
            tmp.add(a);
            userToTweetId.put(userId, tmp);
        } else {
            userToTweetId.get(userId).add(a);
        }
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res = new LinkedList<>();
        PriorityQueue<t> p = new PriorityQueue<>( (a,b) -> b.time-a.time);
        if(userToTweetId.get(userId)!=null){
        for (t id : userToTweetId.get(userId)) {
            p.add(id);
        }
        }
        if (followerToFollowee.get(userId)!=null&&followerToFollowee.get(userId).size() > 0) {
            for (int f : followerToFollowee.get(userId)) {
                if(userToTweetId.get(f)==null) continue;
                for (t id : userToTweetId.get(f)) {
                    p.add(id);
                }
            }
        }
        int count = 0;
        while (!p.isEmpty() && count < 10) {
            res.add(p.poll().id);
            count++;
        }
        return res;
    }

    /**
     * Follower follows a followee. If the operation is invalid, it should be a no-op.
     */
    public void follow(int followerId, int followeeId) {
        if(followerId==followeeId) return;
        if (!followerToFollowee.containsKey(followerId)) {
            Set<Integer> tmp = new HashSet<>();
            tmp.add(followeeId);
            followerToFollowee.put(followerId, tmp);
        } else {
            followerToFollowee.get(followerId).add(followeeId);
        }
    }

    /**
     * Follower unfollows a followee. If the operation is invalid, it should be a no-op.
     */
    public void unfollow(int followerId, int followeeId) {
        if(followerToFollowee.get(followerId)==null) return;
        if (followerToFollowee.get(followerId).contains(followeeId)) {
            followerToFollowee.get(followerId).remove(followeeId);
        }
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

## Solution Map+Set+链表

* 维护每个用户发的推特的链表头，取是个最近的类似 合并k个链表

```java
class Twitter {
    class Tweet{
        int id;
        int time=0;
        Tweet next;
        public Tweet(int id, int time){
            this.id=id;
            this.time=time;
            next=null;
        }
    }
    /** Initialize your data structure here. */
    HashMap<Integer, Tweet> map;
    HashMap<Integer, HashSet<Integer>> followees;
    int timeStamp;
    public Twitter() {
        map=new HashMap<Integer, Tweet>();
        followees=new HashMap<Integer, HashSet<Integer>>();
        timeStamp=0;
    }
    
    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        Tweet temp=new Tweet(tweetId, timeStamp++);
        //每次最新发的那个tweet，将新发的插在他之前
        Tweet head=map.get(userId);
        temp.next=head;
        head=temp;
        map.put(userId, head);
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res=new ArrayList<Integer>();
        List<Tweet> tweets=new ArrayList<Tweet>();
        if(map.containsKey(userId)) tweets.add(map.get(userId));
        HashSet<Integer> followeeIds=followees.get(userId);
        if(followeeIds!=null){
            for(Integer followeeId: followeeIds){
                if(map.containsKey(followeeId))
                    tweets.add(map.get(followeeId));
            }
        }
        //类似合并k个链表
        for(int i=0; i<10; i++){
            int max_index=-1;
            int max=Integer.MIN_VALUE;
            for(int j=0; j<tweets.size(); j++){
                Tweet temp=tweets.get(j);
                if(temp==null) continue;
                if(temp.time>max){
                    max=temp.time;
                    max_index=j;
                }
            }
            if(max_index>=0){
                res.add(tweets.get(max_index).id);
                tweets.set(max_index, tweets.get(max_index).next);
            }
        }
        return res;
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        if(followerId==followeeId) return;
        HashSet<Integer> followeeIds=followees.get(followerId);
        if(followeeIds==null){
            followeeIds=new HashSet<Integer>();
            followees.put(followerId, followeeIds);
        }
        followeeIds.add(followeeId);
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        HashSet<Integer> followeeIds=followees.get(followerId);
        if(followeeIds==null) return;
        followeeIds.remove(followeeId);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

### Solution 加user对象

```java
public class Twitter {
	private static int timeStamp=0;

	// easy to find if user exist
	private Map<Integer, User> userMap;

	// Tweet link to next Tweet so that we can save a lot of time
	// when we execute getNewsFeed(userId)
	private class Tweet{
		public int id;
		public int time;
		public Tweet next;

		public Tweet(int id){
			this.id = id;
			time = timeStamp++;
			next=null;
		}
	}


	// OO design so User can follow, unfollow and post itself
	public class User{
		public int id;
		public Set<Integer> followed;
		public Tweet tweet_head;

		public User(int id){
			this.id=id;
			followed = new HashSet<>();
			follow(id); // first follow itself
			tweet_head = null;
		}

		public void follow(int id){
			followed.add(id);
		}

		public void unfollow(int id){
			followed.remove(id);
		}


		// everytime user post a new tweet, add it to the head of tweet list.
		public void post(int id){
			Tweet t = new Tweet(id);
			t.next=tweet_head;
			tweet_head=t;
		}
	}




	/** Initialize your data structure here. */
	public Twitter() {
		userMap = new HashMap<Integer, User>();
	}

	/** Compose a new tweet. */
	public void postTweet(int userId, int tweetId) {
		if(!userMap.containsKey(userId)){
			User u = new User(userId);
			userMap.put(userId, u);
		}
		userMap.get(userId).post(tweetId);

	}



	// Best part of this.
	// first get all tweets lists from one user including itself and all people it followed.
	// Second add all heads into a max heap. Every time we poll a tweet with 
	// largest time stamp from the heap, then we add its next tweet into the heap.
	// So after adding all heads we only need to add 9 tweets at most into this 
	// heap before we get the 10 most recent tweet.
	public List<Integer> getNewsFeed(int userId) {
		List<Integer> res = new LinkedList<>();

		if(!userMap.containsKey(userId))   return res;

		Set<Integer> users = userMap.get(userId).followed;
		PriorityQueue<Tweet> q = new PriorityQueue<Tweet>(users.size(), (a,b)->(b.time-a.time));
		for(int user: users){
			Tweet t = userMap.get(user).tweet_head;
			// very imporant! If we add null to the head we are screwed.
			if(t!=null){
				q.add(t);
			}
		}
		int n=0;
		while(!q.isEmpty() && n<10){
		  Tweet t = q.poll();
		  res.add(t.id);
		  n++;
		  if(t.next!=null)
			q.add(t.next);
		}

		return res;

	}

	/** Follower follows a followee. If the operation is invalid, it should be a no-op. */
	public void follow(int followerId, int followeeId) {
		if(!userMap.containsKey(followerId)){
			User u = new User(followerId);
			userMap.put(followerId, u);
		}
		if(!userMap.containsKey(followeeId)){
			User u = new User(followeeId);
			userMap.put(followeeId, u);
		}
		userMap.get(followerId).follow(followeeId);
	}

	/** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
	public void unfollow(int followerId, int followeeId) {
		if(!userMap.containsKey(followerId) || followerId==followeeId)
			return;
		userMap.get(followerId).unfollow(followeeId);
	}
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```


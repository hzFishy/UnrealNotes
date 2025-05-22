Full details [here](https://www.reddit.com/r/Unity3D/comments/eye8yk/performance_results_collider_vs_overlap_test/)

> [!quote] TLDR
> *"We showed that checking a sphere overlap every frame is worse then just using a trigger collider.
> We were able to come up with a solution that showed performance improvement vs a collider, but only when we decided not to care about the exact frame something collides. This was most noticeable for the 500 entities test, however the 100 entity version only saw modest changes."*

500 entities with trigger collider:
![[Pasted image 20250522142133.png]]

500 entities with overlap test:
![[Pasted image 20250522142153.png]]

500 entities with overlap test spread randomly across multiple frames
![[Pasted image 20250522142225.png]]


- [What setting the MAPROOT does is that it maps the root user of the machine accessing the freenas to an non-root user on the freenas box.](https://www.truenas.com/community/threads/mapall-maproot-better-explanation-please.54877/)
- Look into maproot or mapall. Maproot maps the remote root user to a single local user. Mapall maps all non-root users to a single local user.
- [For  `root_squash` , use  `-maproot="nobody":"nobody"`; 
  For  `no_root_squash` , use  `-maproot="root":"wheel"`](https://www.truenas.com/community/threads/equivalent-for-root_squash-no_root_squash-on-freenas.80024/)
-
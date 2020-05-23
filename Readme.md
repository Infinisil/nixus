# Nixus: Deployment tool for multiple NixOS systems

This is a work-in-progress deployment tool I'm developing for myself and [Niteo](https://niteo.co/). Noteworthy features include:

- Auto-rollback if the machine can't be reached anymore, protecting against a number of configuration mistakes such as
  - Messing up the network config
  - Removing your SSH key from the authorized keys
  - The activation script failing in any way
  - The boot activation failing in any way
  - The system crashing during the deployment
- The ability to write multi-host abstractions (to be documented)
- Secret management
- More coming..

## How to use it

Note: This is just to demonstrate, this will probably change in the future

Write a file like `example/default.nix`, then build the deployment script and call it
```
$ nix-build example/default.nix
these derivations will be built:
  /nix/store/lv8ck2k8b6vmsdp8wlqlpqr4shbkplfa-system-units.drv
  /nix/store/azyfd4qhv2hcdagcr8hmzwa2q284f9rh-etc.drv
  /nix/store/3kzhmi0flgcnpn6s5rym6hv8rs48hrs2-nixos-system-test-20.03pre-git.drv
  /nix/store/q6qx69mzy50llv3i7by5wwqyirqhpijy-deploy-foo.example.com.drv
  /nix/store/l7di8hzwa1m784ycqw01hdrybaxdi1jw-deploy.drv
building '/nix/store/lv8ck2k8b6vmsdp8wlqlpqr4shbkplfa-system-units.drv'...
building '/nix/store/azyfd4qhv2hcdagcr8hmzwa2q284f9rh-etc.drv'...
building '/nix/store/3kzhmi0flgcnpn6s5rym6hv8rs48hrs2-nixos-system-test-20.03pre-git.drv'...
building '/nix/store/q6qx69mzy50llv3i7by5wwqyirqhpijy-deploy-foo.example.com.drv'...
building '/nix/store/l7di8hzwa1m784ycqw01hdrybaxdi1jw-deploy.drv'...
/nix/store/z73pjq6d7n6f3xfhx9rycfk9sxqjmcav-deploy
$ ./result
[foo.example.com] Connecting to host...
[foo.example.com] Copying closure to host...
[foo.example.com] copying 3 paths...
[foo.example.com] copying path '/nix/store/f1028ijc3c2654z8ikzd378ryp644h3f-system-units' to 'ssh://root@138.68.83.114'...
[foo.example.com] copying path '/nix/store/9py44f4x9m83pr3j93c1fs95p0qy6175-etc' to 'ssh://root@138.68.83.114'...
[foo.example.com] copying path '/nix/store/8hbnksxrhgwpmia833xp8191a5yxw8ii-nixos-system-test-20.03pre-git' to 'ssh://root@138.68.83.114'...
[foo.example.com] Triggering system switcher...
[foo.example.com] Trying to confirm success...
[foo.example.com] Successfully activated new system!
```

Here is an example of a messed up network config:
```
[foo.example.com] Connecting to host...
[foo.example.com] Copying closure to host...
[foo.example.com] copying 3 paths...
[foo.example.com] copying path '/nix/store/dh08694j23zbp6rra8wbhr9yy4vri49h-system-units' to 'ssh://root@138.68.83.114'...
[foo.example.com] copying path '/nix/store/xyslp1r2267vsrlrq73h79w31p2na223-etc' to 'ssh://root@138.68.83.114'...
[foo.example.com] copying path '/nix/store/3ndywy808vm6ahbwkmam4sqvxy0hv7hq-nixos-system-test-20.03pre-git' to 'ssh://root@138.68.83.114'...
[foo.example.com] Triggering system switcher...
[foo.example.com] Trying to confirm success...
[foo.example.com] Failed to activate new system! Rolled back to previous one
```

# custom_coredns

## Testing
To test you can run the below samples against your coredns, assuming the default configurations I've customised are used.

```bash
dig @127.0.0.1 -p 10053 ip-172-55-55-66.us-west-1.compute.internal
dig @127.0.0.1 -p 10053 CH version.bind TXT
dig @127.0.0.1 -p 10053 example.org

```

## Future plans?
* This example currently uses **file** for the zones but can be flipped to **redis** and **sql** in the future if I decide I want to build my own coredns binary.

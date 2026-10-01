# Verification Checklist

## PC4

```text
IP: 192.168.10.10
Mask: 255.255.255.0
Gateway: 192.168.10.1
```

Test:

```text
ping 192.168.20.10
```

## PC5

```text
IP: 192.168.20.10
Mask: 255.255.255.0
Gateway: 192.168.20.1
```

Test:

```text
ping 192.168.10.10
```

## Expected result

Both pings should succeed after all interfaces are enabled and the static routes are configured correctly.

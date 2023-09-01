# WASI-Virt Encapsulation Case

Setup: `setup.sh` installs cargo nightly, wasi virt from git.
Virt test: `test.sh` does a virtualization.

### Result

A virtualized component `greet-wasi.py.virt.wasm` is created, with risidual type only imports to the network subsystem:

```
wasm-tools print greet-wasi.py.virt.wasm -o virt.wat
```

Gives:

```
(component
  (type (;0;)
    (instance
      (type (;0;) (func (result string)))
      (export (;0;) "greet" (func (type 0)))
    )
  )
  (import (interface "wasmcon2023:greet/interface") (instance (;0;) (type 0)))
  (type (;1;)
    (instance
      (type (;0;) u32)
      (export (;1;) "network" (type (eq 0)))
      (type (;2;) (enum "unknown" "access-denied" "not-supported" "out-of-memory" "timeout" "concurrency-conflict" "not-in-progress" "would-block" "address-family-not-supported" "address-family-mismatch" "invalid-remote-address" "ipv4-only-operation" "ipv6-only-operation" "new-socket-limit" "already-attached" "already-bound" "already-connected" "not-bound" "not-connected" "address-not-bindable" "address-in-use" "ephemeral-ports-exhausted" "remote-unreachable" "already-listening" "not-listening" "connection-refused" "connection-reset" "datagram-too-large" "invalid-name" "name-unresolvable" "temporary-resolver-failure" "permanent-resolver-failure"))
      (export (;3;) "error-code" (type (eq 2)))
      (type (;4;) (tuple u8 u8 u8 u8))
      (export (;5;) "ipv4-address" (type (eq 4)))
      (type (;6;) (tuple u16 u16 u16 u16 u16 u16 u16 u16))
      (export (;7;) "ipv6-address" (type (eq 6)))
      (type (;8;) (variant (case "ipv4" 5) (case "ipv6" 7)))
      (export (;9;) "ip-address" (type (eq 8)))
      (type (;10;) (enum "ipv4" "ipv6"))
      (export (;11;) "ip-address-family" (type (eq 10)))
      (type (;12;) (record (field "port" u16) (field "address" 5)))
      (export (;13;) "ipv4-socket-address" (type (eq 12)))
      (type (;14;) (record (field "port" u16) (field "flow-info" u32) (field "address" 7) (field "scope-id" u32)))
      (export (;15;) "ipv6-socket-address" (type (eq 14)))
      (type (;16;) (variant (case "ipv4" 13) (case "ipv6" 15)))
      (export (;17;) "ip-socket-address" (type (eq 16)))
    )
  )
  (import (interface "wasi:sockets/network") (instance (;1;) (type 1)))
  (component (;0;)
  ...
_
```

Where the `wasi:sockets/network` import should not be present.
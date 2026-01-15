Known Pitfalls and Solutions
1. Kernel Version Matters

Problem: Stock Jetson kernel (5.15) doesn't support PQC/additional key exchange in Child SA.

Solution: Use kernel 6.1+ (we provide 6.6 in our BSP). The XFRM subsystem added XFRM_MSG_NEWAE for additional key exchange in Linux 6.1.
2. liboqs Plugin Loading

Problem: strongSwan says "plugin 'oqs' failed to load"

Solution:

# Ensure liboqs is in the library path
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
ldconfig

# Verify dependencies
ldd /usr/lib/ipsec/plugins/libstrongswan-oqs.so

3. BLAST Plugin Dependencies

Problem: "libQryptSecurityC.so: cannot open shared object file"

Solution:

# Copy Qrypt libraries to system path
cp /usr/local/lib/libQrypt*.so* /usr/lib/
ldconfig

4. Kyber / Blast not loading

Problem: "ERROR: algorithm from private space would match, but peer implementation is unknown"

Solution:

# Add to /etc/strongswan.conf:
charon {
    accept_private_algs = yes
    plugins {
        oqs {
            load = yes
        }
    }
}


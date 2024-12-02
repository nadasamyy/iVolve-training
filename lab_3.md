# Lab 3: Create a shell script to ping every server in the 172.16.17.x subnet (where x is a number between 0 and 255). If the ping succeeds, display the message “Server 172.16.17.x is up and running” If the ping fails, display the message “Server 172.16.17.x is unreachable”.


```bash
#!/bin/bash

# Configuration
SUBNET="172.16.17"
TIMEOUT=1
COUNT=1
PARALLEL=10  # Number of parallel processes

# Function to ping a single IP
ping_ip() {
    local IP=$1
    if ping -c $COUNT -W $TIMEOUT $IP >/dev/null 2>&1; then
        echo "✓ Server $IP is up and running"
    else
        echo "✗ Server $IP is unreachable"
    fi
}

# Main script
echo "Starting network scan for subnet $SUBNET.0/24"
echo "Timeout: ${TIMEOUT}s, Ping count: $COUNT"
echo "----------------------------------------"

# Start time
start_time=$(date +%s)

# Export functions and variables for parallel execution
export -f ping_ip
export TIMEOUT COUNT

# Use parallel processing
for i in {0..255}; do
    IP="$SUBNET.$i"
    ping_ip "$IP" &
    
    # Limit parallel processes
    running=$(jobs -r | wc -l)
    while [ $running -ge $PARALLEL ]; do
        sleep 0.1
        running=$(jobs -r | wc -l)
    done
done

# Wait for all background processes to complete
wait

# End time
end_time=$(date +%s)
duration=$((end_time - start_time))

echo "----------------------------------------"
echo "Scan completed in $duration seconds"'
```

FROM trufflesuite/ganache:latest

# Expose the standard port used in your application
EXPOSE 7545

# Start Ganache on port 7545 (0.0.0.0 is automatic in this docker image)
# We inject the exact private key used in your Spring Boot application with 1000 ETH
CMD ["--server.port", "7545", "--wallet.accounts=0xe0396dd66930d63f462627faa75345913890d5476f4441b2bfb639a8a838f51f,1000000000000000000000"]

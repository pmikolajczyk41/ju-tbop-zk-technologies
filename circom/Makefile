.PHONY: compile-circuit
compile-circuit: ## Compile the circuit
	@circom main.circom --r1cs --wasm --sym

.PHONY: generate-witness
generate-witness: ## Generate a witness for the circuit
	@cd main_js && node generate_witness.js main.wasm ../input.json ../witness.wtns

.PHONY: setup
setup: ## Run Groth16 trusted setup for the circuit
	@snarkjs powersoftau new bn128 12 pot12_0000.ptau -v
	@snarkjs powersoftau contribute pot12_0000.ptau pot12_0001.ptau --name="First Contributor" -v
	@snarkjs powersoftau prepare phase2 pot12_0001.ptau pot12_final.ptau -v
	@snarkjs groth16 setup main.r1cs pot12_final.ptau main_0000.zkey
	@snarkjs zkey contribute main_0000.zkey main_0001.zkey --name="First Contributor" -v
	@snarkjs zkey export verificationkey main_0001.zkey verification_key.json

.PHONY: generate-proof
generate-proof: ## Generate a proof for the circuit
	@snarkjs groth16 prove main_0001.zkey witness.wtns proof.json public.json

.PHONY: verify
verify: ## Verify the proof
	@snarkjs groth16 verify verification_key.json public.json proof.json

.PHONY: clean
clean: ## Clean the generated files
	@rm -rf main.r1cs main.sym main_js/ witness.wtns pot12_*.ptau main_*.zkey verification_key.json proof.json public.json

.PHONY: help
help: ## Displays this help
	@awk 'BEGIN {FS = ":.*##"; printf "Usage:\n  make \033[1;36m<target>\033[0m\n\nTargets:\n"} /^[a-zA-Z0-9_-]+:.*?##/ { printf "  \033[1;36m%-25s\033[0m %s\n", $$1, $$2 }' $(MAKEFILE_LIST)
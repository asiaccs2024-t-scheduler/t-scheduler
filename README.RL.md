# Fuzzing with Reinforcement Learning

If building with the C++ RL code, install Boost:

```bash
sudo apt install -y libboost-all-dev
```

Build AFL++:

```bash
make RL_FUZZING=1
```

If building with the Python RL code, use:

```bash
make PY_RL_FUZZING=1
```

Then configure a Python virtualenv (only required if using Python RL):

```bash
python3 -m venv venv
source venv/bin/activate
pip3 install --upgrade pip
pip3 install -r RL-requirements.txt
```

Start the Python service:

```bash
./src/RLFuzzing.py
```

Start the fuzzer:

```bash
export AFL_SKIP_CPUFREQ=1
export AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1
export AFL_TESTCACHE_SIZE=2

# without rareness : 0
# with rareness : 1
# with rareness and sqrt : 2
# sample rareness : 3
# rare without reinforcement learning : 4
export AFL_RL_CORRECTION_FACTOR=0

./afl-fuzz -i /out/seeds -o /out/corpus -m none -t 1000+ -d -Q -c0 -- /out/fuzz_target 2147483647
```

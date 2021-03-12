BootStrap: docker
#From: continuumio/miniconda3:latest
From: debian

%labels
MAINTAINER Jesus Murga Moreno

%environment

	export LC_ALL=C.UTF-8
	export LANG=C.UTF-8
	export PATH=/relate_v1.1.6_x86_64_static/bin:/relate_v1.1.6_x86_64_static/scripts/DetectSelection:/relate_v1.1.6_x86_64_static/scripts/EstimatePopulationSize:/relate_v1.1.6_x86_64_static/scripts/PrepareInputFiles:/relate_v1.1.6_x86_64_static/scripts/RelateParallel:$PATH
	export PATH=/opt/conda/bin:$PATH
	export PATH=/ABCreg/src/:$PATH

	export PATH=/julia-1.5.3/bin/:$PATH
	export JULIA_DEPOT_PATH=:/opt/.julia
	
%post
	
	export JULIA_DEPOT_PATH=/opt/.julia

	apt-get update && apt-get install -y --no-install-recommends apt-utils
	apt-get install -y libglib2.0-0 libxext6 wget bzip2 ca-certificates curl git vim make build-essential libgsl-dev libz-dev gzip bcftools samtools vcftools
	apt-get clean
	rm -rf /var/lib/apt/lists/*

	wget https://raw.githubusercontent.com/jmurga/Analytical.jl/master/scripts/precompile_mktest.jl
	wget https://raw.githubusercontent.com/jmurga/Analytical.jl/master/scripts/abcmk_cli.jl

	export PATH=/opt/conda/bin:$PATH

	# conda install -c conda-forge numpy pandas ipython scipy scikit-allel numba msprime pyslim tskit slim -y
	# conda install -c conda-forge numpy pandas ipython scipy scikit-allel numba msprime pyslim tskit slim r-base r-dplyr r-data.table r-popgenome r-rmysql r-stringr r-dbi parallel -y

	# conda install -c bioconda selscan hapbin gatk4 bedtools -y
	# conda install -c genomedk polydfe grapes-static
	# conda clean --all

	# /opt/conda/bin/python -m pip install tsinfer

	wget https://julialang-s3.julialang.org/bin/linux/x64/1.5/julia-1.5.3-linux-x86_64.tar.gz
	tar -zxf julia-1.5.3-linux-x86_64.tar.gz
	# git clone https://github.com/molpopgen/ABCreg.git /ABCreg
	# make -C /ABCreg/src/

	/julia-1.5.3/bin/julia -e 'using Pkg;Pkg.add(["Fire","PackageCompiler" ,"Parameters", "NLsolve", "SpecialFunctions", "Distributions", "Roots", "ArbNumerics", "StatsBase", "LsqFit", "PoissonRandom", "SparseArrays", "Distributed", "CSV", "SharedArrays", "JLD2","DataFrames","GZip","Parsers","OrderedCollections","FastaIO","RCall","ClusterManagers"])'

	/julia-1.5.3/bin/julia -e 'using Pkg;Pkg.add(PackageSpec(path="https://github.com/jmurga/Analytical.jl"))'

	/julia-1.5.3/bin/julia -e 'using PackageCompiler;PackageCompiler.create_sysimage(:Analytical, sysimage_path="/mktest.so", precompile_execution_file="/precompile_mktest.jl")'

	wget https://myersgroup.github.io/relate/download/relate_v1.1.6_x86_64_static.tgz
	tar -zxf relate_v1.1.6_x86_64_static.tgz
	rm relate_v1.1.6_x86_64_static.tgz

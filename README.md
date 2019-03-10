## About
Repository contains code for running simulations published in [ Hnisz et al., 2017](https://doi.org/10.1016/j.cell.2017.02.007).

## Running the code

MATLAB is required to run the program. The main simulation file is  submit_cycle_job.m.

Broadly, the mean field simulations model systems of chains, which upon reversible modifications, can form weak cross-linking bonds. (De)modification reactions are catalyzed by (de)modifier enzymes, whose concentrations are parameters of the model.

### Key parameters
  - params.Nc	: Number of chains
  - params.f	: Number of modifiable sites per chain
  - params.kcr_on	: Cross-link on-rate
  - params.kcr_off	: Cross-link off-rate
  - params.Mod	: Concentration of modifiers
  - params.Demod	: Concentration of demodifiers
  - params.k_mod	: On-rate for modifications
  - params.k_demod	: On-rate for modifications
  - params.t_end	: Time for simulations


### Running modifier/demodifier flips
- params.change_flag	: Binary parameter which toggles whether you want flip or not (0 for no flip, 1 for flip)
- params.Mod_change	: If flag is on, this is the new value of Modifier after t_change
- params.t_change		: Time at which flip in modifier levels happen

### Functions/Modules

`[t,cluster_max_size,no_of_clusters,Str,Val] = cyclic_clusters(params,Ntraj)
`

This function is called from submit_job() and it takes in the simulation parameters, and returns the following variables:

- t: Cell vector of time trajectory for each replicate
- cluster_max_size: Cell vector of largest cluster for each trajectory
- no_of_clusters: Cell vector of number of clusters for each trajectory
- Str: Adjacency matrix of cluster connections from the last trajectory
- Val: Valency status of cluster connections at last trajectory

#### Gillespie code for reaction events

`[Str,Val,deltaT] = cyclic_reaction_cluster(Str,Val,params)`

Takes in:
- Str: adjacency matrix showing how chains are connected,
- Val: modification status of every chain
- params: reaction parameters

and returns:

- Str: Adjacency matrix updated after a reaction event
- Val: Updated modification status for all chains after a reaction event
- deltaT: Time takes before the subsequent reaction event occurs


#### Visualization
The visualisation file is visualisation_data.m

This takes in the filename of the matlab file where the standard output data is stored and plots the mean cluster size (+-2 sigma) over multiple trajectories.

\* if N is the
sweeping variable, then the output has to be rescaled by
Nc, rather than params.Nc.

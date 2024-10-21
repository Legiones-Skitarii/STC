# Denoising Diffusion Probabilistic Models

* [Repo](Denoising Diffusion Probabilistic Models)

*
``` 
Real Data -> [Add Noise] -> Noisy Data -> [Diffusion Model] -> Generated Data
```

* Markov chains
	* Current event only depends on previous event
	* a -> b -> c
* Guassian noise
	* f(x) = (1/z)*e^(z˚)
		* z = √(2*π*σ)
		* z˚ = -((z-µ)^2)/(2*(σ^2))
	* guassian disturbution
		* sigma controls the peak of the curve. lesser the sigma, higher the peak.
		* µ controls the x coordinate of the peak. -ve pulls it towards center, + pushes it away.
	* Inflection points -> µ - sigma : µ + sigma

	* Total area under the curve is 1 -> Probability disturbution


----

### Links
* https://yang-song.net/blog/2021/score/
* https://lilianweng.github.io/posts/2021-07-11-diffusion-models/
* https://github.com/Ryota-Kawamura/How-Diffusion-Models-Work/blob/main/L1_Sampling.ipynb
* https://github.com/dome272/Diffusion-Models-pytorch/blob/main/ddpm_conditional.py
* https://spraphul.github.io/blog/diffusion-models#the-diffusion-process 
* https://arxiv.org/pdf/2208.11970
* https://theaisummer.com/diffusion-models/
* https://theaisummer.com/latent-variable-models/#reparameterization-trick
* https://lilianweng.github.io/posts/2018-10-13-flow-models/
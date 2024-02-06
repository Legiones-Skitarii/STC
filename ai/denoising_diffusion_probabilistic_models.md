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



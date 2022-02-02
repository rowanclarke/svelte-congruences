<script lang="ts">
	import Slider from '@smui/slider';
	import FormField from '@smui/form-field';

	let a = 8;
	let b = 6;
	let m = 15;
	let n = 10;

	$:res = euclid(m, n);
	$:cong = res !== undefined;
	
	$:[r, s] = cong ? res : [0, 0];
	$:gcd = r * m + s * n;
	$:lcm = m * n / gcd;
	$:x = ((((a*s*n + b*r*m) / gcd) % lcm) + lcm) % lcm;

	function euclid(m: number, n: number): [number, number] | undefined {
		if (n > m) return euclid(n, m).reverse() as [number, number];
		if (m == 0 || n == 0) return undefined;
		let q = Math.floor(m / n);
		let r = m % n;
		if (r == 0) return [1, 1 - q];
		if (n % r == 0) return [1, -q];
		let [a, b] = euclid(n, r);
		return [b, a - q * b];
	}
</script>

<div id="cong">
	<h1>Congruences</h1>
	<pre>
This is about the most interesting topic of my course so far.

<i>x</i> is said to be <b>congruent</b> (≡) to <i>a</i> <b>mod</b> <i>m</i> if <i>m</i> is a factor of <i>x</i> - <i>a</i>.
In other words, there exists an integer <i>q</i> where <i>x</i> = <i>m</i><i>q</i> + <i>a</i>.
We can find all solutions for <i>x</i> by going though all integers of <i>q</i>.

Now we try to solve simultaneous congruences.
	</pre>
	<h3>Solve the simultaneous congruences</h3>
	<div style="display:flex;">
		<div>
			<FormField style="display: flex;">
				<span	slot="label" class="label">a = {a}</span>
				<Slider class="slider" bind:value={a} />
			</FormField>
			<FormField style="display: flex;">
				<span	slot="label" class="label">b = {b}</span>
				<Slider class="slider" bind:value={b} />
			</FormField>
			<FormField style="display: flex;">
				<span	slot="label" class="label">m = {m}</span>
				<Slider class="slider" bind:value={m} min={1} max={300}/>
			</FormField>
			<FormField style="display: flex;">
				<span	slot="label" class="label">n = {n}</span>
				<Slider class="slider" bind:value={n} min={1} max={300}/>
			</FormField>
		</div>
		<div style="padding-left: 100px;">
			<pre>
x ≡ {a} mod {m}
x ≡ {b} mod {n}

gcd({m}, {n}) = ({r})*{m} + ({s})*{n} = {gcd}
</pre>
{#if (a-b) % gcd == 0}
<pre>

There is a solution because {gcd} is a factor of {a} - {b} = {a-b}

lcm({m}, {n}) = ({m}*{n}) / gcd({m}, {n}) = {lcm}
x ≡ (({a}*{s}*{n} + {b}*{r}*{m}) / gcd({m}, {n})) mod lcm({m}, {n})
so
x ≡ {(a*s*n + b*r*m) / gcd} mod {lcm}
so
x ≡ {x} mod {lcm}
			</pre>
			{:else}
			<pre>There is no solution because {gcd} is not a factor of {a} - {b} = {a-b}</pre>
			{/if}
		</div>
	</div>
</div>
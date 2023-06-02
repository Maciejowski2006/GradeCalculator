<script lang="ts">
    import {createForm} from "svelte-forms-lib";

    let final = 0;

    const {form, handleChange, handleReset} = createForm({
        initialValues: {
            grades: [
                {
                    grade: 0,
                    weight: 0
                }
            ]
        },
        onSubmit: null
    });

    const add = () => {
        $form.grades = $form.grades.concat({grade: 0, weight: 0});
    };

    const remove = i => () => {
        $form.grades = $form.grades.filter((u, j) => j !== i);
    };

    const calculateFinal = () => {
        let sumGrades = 0;
        let sumWeights = 0;

        $form.grades.forEach(v => {
			sumGrades += v.grade * v.weight;
            sumWeights += v.weight;
        });
        final = sumGrades / sumWeights;
    }

</script>
<section class="flex flex-col-reverse gap-x-8">
	<form on:change={calculateFinal}>
		{#each $form.grades as user, j}
			<div class="form-group">
				<div class="flex flex-row gap-4 p-2">
					<div>
						<label class="label">
							<span class="label-text">Ocena</span>
						</label>
						<input
								class="input input-bordered w-full max-w-xs drop-shadow-xl"
								name={`users[${j}].name`}
								type="number"
								on:change={handleChange}
								on:blur={handleChange}
								bind:value={$form.grades[j].grade}
						/>

					</div>
					<div>
						<label class="label">
							<span class="label-text">Waga</span>
						</label>
						<input
								class="input input-bordered w-full max-w-xs drop-shadow-xl"
								type="number"
								name={`users[${j}].email`}
								on:change={handleChange}
								on:blur={handleChange}
								bind:value={$form.grades[j].weight}
						/>

					</div>

					{#if $form.grades.length !== 1}
						<button type="button" class="btn drop-shadow-xl mt-[36px]" on:click={remove(j)}>-</button>
					{:else}
						<button type="button" class="btn drop-shadow-xl mt-[36px]" disabled>-</button>
					{/if}
				</div>
			</div>
		{/each}

		<div class="flex flex-row gap-x-4 justify-center mt-4 mx-2 flex-grow flex-1">
			<button type="button" class="btn drop-shadow-xl grow" on:click={add}>+</button>
			<button type="button" class="btn drop-shadow-xl grow" on:click={handleReset}>reset</button>
		</div>
	</form>
	<div class="w-full">
		<span class="block text-center w-full">
			Przewidywana średnia: { final.toFixed(2) }
		</span>
	</div>
</section>

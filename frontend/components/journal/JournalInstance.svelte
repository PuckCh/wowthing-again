<script lang="ts">
    import find from 'lodash/find'

    import { journalStore } from '@/stores'
    import { journalState } from '@/stores/local-storage'
    import type { JournalDataInstance, JournalDataTier } from '@/types/data'

    import CheckboxInput from '@/components/forms/CheckboxInput.svelte'
    import Group from './JournalGroup.svelte'
    import SectionTitle from '@/components/collections/CollectionSectionTitle.svelte'

    export let slug1: string
    export let slug2: string

    let instance: JournalDataInstance
    $: {
        instance = undefined
        const tier: JournalDataTier = find($journalStore.data.tiers, (tier) => tier.slug === slug1)
        if (tier) {
            instance = find(tier.instances, (instance) => instance.slug === slug2)
        }
    }
</script>

<style lang="scss">
    .wrapper {
        width: 100%;
    }
</style>

<div class="wrapper">
    <div class="options-container">
        <button>
            <CheckboxInput
                name="highlight_missing"
                bind:value={$journalState.highlightMissing}
            >Highlight missing</CheckboxInput>
        </button>

        <span>Show:</span>

        <button>
            <CheckboxInput
                name="show_collected"
                bind:value={$journalState.showCollected}
            >Collected</CheckboxInput>
        </button>

        <button>
            <CheckboxInput
                name="show_uncollected"
                bind:value={$journalState.showUncollected}
            >Missing</CheckboxInput>
        </button>

        <button class="margin-left">
            <CheckboxInput
                name="show_cloth"
                bind:value={$journalState.showCloth}
            >Cloth</CheckboxInput>
        </button>

        <button>
            <CheckboxInput
                name="show_leather"
                bind:value={$journalState.showLeather}
            >Leather</CheckboxInput>
        </button>

        <button>
            <CheckboxInput
                name="show_mail"
                bind:value={$journalState.showMail}
            >Mail</CheckboxInput>
        </button>

        <button>
            <CheckboxInput
                name="show_plate"
                bind:value={$journalState.showPlate}
            >Plate</CheckboxInput>
        </button>

        <button>
            <CheckboxInput
                name="show_weapons"
                bind:value={$journalState.showWeapons}
            >Weapons</CheckboxInput>
        </button>

        <button class="margin-left">
            <CheckboxInput
                name="show_timewalking"
                bind:value={$journalState.showTimewalking}
            >Timewalking</CheckboxInput>
        </button>
    </div>

    {#if instance}
        <div class="collection thing-container">
            {#each instance.encounters as encounter}
                <SectionTitle
                    title={encounter.name}
                    count={$journalStore.data.stats[`${slug1}--${slug2}--${encounter.name}`]}
                />
                <div class="collection-section">
                    {#each encounter.groups as group}
                        <Group
                            bonusIds={instance.bonusIds}
                            stats={$journalStore.data.stats[`${slug1}--${slug2}--${encounter.name}--${group.name}`]}
                            {group}
                        />
                    {/each}
                </div>
            {/each}
        </div>
    {/if}
</div>

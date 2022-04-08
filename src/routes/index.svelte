<script>
    import currency from 'currency.js';
    import 'normalize.css';
    import '../app.css';

    let bill = '';
    let percent = 0;
    let people = '';
    let tip = '';
    let amount = 0;
    let total = 0;

    const radios = [
        '5%',
        '10%',
        '15%',
        '25%',
        '50%'
    ];

    function handleClick() {
        tip = parseInt(this.value);

        if (!people || !bill || !tip) return;

        const percentage = bill / 100 * tip;
        amount = percentage / people;
        total = bill / people;
    }

    function handleRadios() {
        emptyCustomTipField();

        tip = parseInt(this.value.replace('%', ''));

        if (!people || !bill || !tip) return;

        const percentage = bill / 100 * tip;
        amount = percentage / people;
        total = bill / people;
    }

    function handleCustomTipKeyup() {
        tip = parseInt(this.value);

        if (!people || !bill || !tip) return;

        const percentage = bill / 100 * tip;
        amount = percentage / people;
        total = bill / people;
    }

    function handleInput() {
        if (!people || !bill || !tip) {
            amount = 0;
            total = 0;
            return;
        }

        const percentage = bill / 100 * tip;
        amount = percentage / people
        total = bill / people;
    }

    function emptyCustomTipField() {
        document.getElementById('custom').value = '';
    }

    function uncheckRadios() {
        const radio = document.querySelector('input[name="tip"]:checked');
        if (radio) radio.checked = false;
    }

    function focusBillField() {
        document.getElementById('bill').focus();
    }

    function handleReset() {
        bill = '';
        people = '';
        amount = 0;
        total = 0;

        uncheckRadios();
        emptyCustomTipField();
        focusBillField();
    }
</script>

<header class="header">
    <h1 class="logo">
        <img src="/logo.svg" alt="Splitter" width="87" height="54">
    </h1>
</header>

<main class="container">
    <form class="form">
        <div class="form-group">
            <label for="bill" class="form-label">Bill</label>
            <input type="number" min="1" step="any" name="bill" id="bill" class="form-control form-dollar" aria-invalid="{bill === 0 ? 'true' : 'false'}" bind:value={bill} on:keyup={handleInput}>

            {#if bill === 0}
                <span class="form-message">Can't be zero</span>
            {/if}
        </div>

        <div class="form-group">
            <label for="tip" class="form-label">Select Tip %</label>
            
            <div class="radio-group">
                {#each radios as radio, i}
                    <div class="radio">
                        <input type="radio" name="tip" id="tip_{i}" class="sr-only" on:change={handleRadios} value="{radio}">
                        <label for="tip_{i}" class="btn">{radio}</label>
                    </div>
                {/each}

                <label for="custom" class="sr-only">Custom Tip</label>
                <input type="text" name="custom" id="custom" class="form-control form-custom" placeholder="Custom" on:click={uncheckRadios} on:keyup={handleCustomTipKeyup}>
            </div>
        </div>

        <div class="form-group">
            <label for="people" class="form-label">Number of People</label>
            <input type="number" name="people" id="people" class="form-control form-person" aria-invalid="{people === 0 ? 'true' : 'false'}" bind:value={people} on:keyup={handleInput}>
            
            {#if people === 0}
                <span class="form-message">Can't be zero</span>
            {/if}
        </div>
    </form>

    <div class="card">
        <div class="card-row">
            <p class="card-label">
                <span>Tip Amount</span>
                <small>/ person</small>
            </p>
            <p class="card-money">{currency(amount).format()}</p>
        </div>

        <div class="card-row">
            <p class="card-label">
                <span>Total</span>
                <small>/ person</small>
            </p>
            <p class="card-money">{currency(total).format()}</p>
        </div>

        <button type="button" class="btn btn-strong" disabled={!amount} on:click={handleReset}>Reset</button>
    </div>
</main>

<footer class="attribution">
  Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank" rel="noopener noreferrer">Frontend Mentor</a>. Coded by <a href="https://twitter.com/bwalkermills" target="_blank" rel="noopener noreferrer">Byron</a>.
</footer>

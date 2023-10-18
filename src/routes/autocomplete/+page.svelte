<script lang='ts'>
    import * as FlexSearch from '@akryum/flexsearch-es'

    const NO_JONGSUNG = ''

    const CHOSUNGS = ['ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 'ㅅ', 'ㅆ', 'ㅇ',
        'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ']
    const JOONGSUNGS = ['ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', 'ㅓ', 'ㅔ', 'ㅕ', 'ㅖ', 'ㅗ', 'ㅘ', 'ㅙ', 'ㅚ',
        'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 'ㅡ', 'ㅢ', 'ㅣ']
    const JONGSUNGS = [NO_JONGSUNG, 'ㄱ', 'ㄲ', 'ㄳ', 'ㄴ', 'ㄵ', 'ㄶ', 'ㄷ', 'ㄹ', 'ㄺ', 'ㄻ',
        'ㄼ', 'ㄽ', 'ㄾ', 'ㄿ', 'ㅀ', 'ㅁ', 'ㅂ', 'ㅄ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ']

    const N_CHOSUNGS = 19
    const N_JOONGSUNGS = 21
    const N_JONGSUNGS = 28

    const FIRST_HANGUL = 0xAC00 //'가'
    const LAST_HANGUL = 0xD7A3 //'힣'

    const charToUnicode = function(char: string) {
        if (!char) return null; // Escaping if not exist
        return char.charCodeAt(0)
    }

    const cho_joong_jong_tokenizer = function(str: string) {
        const result: string[] = []
        ;([...str]).forEach(char => {
            const unicode = charToUnicode(char)
            if (unicode === null) return
            if (unicode < FIRST_HANGUL || unicode > LAST_HANGUL)
                return result.push(char)
            else {
                let code = unicode - FIRST_HANGUL
                const jongsung_index = code % N_JONGSUNGS
                code = Math.floor(code / N_JONGSUNGS)
                const joongsung_index = code % N_JOONGSUNGS
                code = Math.floor(code / N_JOONGSUNGS)
                const chosung_index = code

                result.push(CHOSUNGS[chosung_index])
                result.push(JOONGSUNGS[joongsung_index])
                if (JONGSUNGS[jongsung_index]) 
                    result.push(JONGSUNGS[jongsung_index])
            }
        })
        return result
    }

    console.log(cho_joong_jong_tokenizer("John Doe").join(''))
    console.log(cho_joong_jong_tokenizer("한글").join(''))
    console.log(cho_joong_jong_tokenizer("ㄱ 오란다 뎌쉐 큵").join(''))

    const index = FlexSearch.create({
        tokenize: 'forward',
        resolution: 5,
        encode: cho_joong_jong_tokenizer
    });

    index.add(0, "John Doe");
    index.add(1, "John rice");
    index.add(2, "한글");
    index.add(3, "John cape");
    index.add(4, "한 글");
    index.add(5, "고하자");
    
    index.search("하").forEach(id => console.log(id));
    const a = "hel"

</script>

<svelte:head>
    <title>Autocomplete</title>
    <meta name='description' content='Autocomplete test'>
</svelte:head>

<section>
	<h1>
		{a}
	</h1>
</section>
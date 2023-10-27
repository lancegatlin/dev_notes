

Convert "param = ???" CSL to NSL
Find: `, `
Replace: `,\n`


Convert "param = ???" NSL to "param = param"
Find: `(\s*)(\w+) = \?\?\?(,*)`
Replace: `$1$2 = $2$3`



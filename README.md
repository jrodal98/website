# Personal Portfolio and Resume

## Portfolio

My portfolio is based on a template released under the MIT license that I found [here](https://github.com/mmacneil/devfolio).  I made a few modifications, such as adding a Github activity tracker and a skills sections, that you might be interested in as well.  Currently, my website is located [here](https://www.jrodal.dev/).

## Resume

My resume is based on a template released under the MIT license that I found [here](https://github.com/dnl-blkv/mcdowell-cv).  A copy of my resume can be found [here](docs/Resume.pdf).

## Githooks

If you're copying my setup, you may also be interested in my `pre-push` githook, which uploads my resume to Dropbox whenever I push a change to Github.  It used to also deploy my website to the University of Virginia Computer Science department web servers, but I no longer host my website there.  Run `git config core.hooksPath .githooks` to "enable" the `pre-push` githook and then edit the `pre-push` executable as desired.
# powerline-term-config
Configure a Linux system for powerline

## Background

This configures a Linux system the way I have my Linux system set up,
with Janus, Vim, tmux, oh-my-zsh, and powerline.

## Installation

On Debian 10
```bash
apt update
apt install vim-nox powerline zsh tmux git rake
```

### Install oh-my-zsh
https://ohmyz.sh/#install
```bash
echo "if [[ -r /usr/share/powerline/bindings/zsh/powerline.zsh ]]; then" >> ~/.zshrc
echo "  source /usr/share/powerline/bindings/zsh/powerline.zsh" >> ~/.zshrc
echo "fi" >> ~/.zshrc
echo ". /usr/share/powerline/bindings/zsh/powerline.zsh" >> ~/.zshrc
exit
```
then exit and log back in, zsh and oh-my-zsh with powerline should be
installed

### Configure Tmux
add the following to ~/.tmux.conf
```conf
set-option -g default-shell $SHELL
#set-option -g default-command "reattach-to-user-namespace -l ${SHELL}"
#set-option -g default-path "$PWD"
setw -g mode-keys vi
set-option -g history-limit 10000
set-option -g repeat-time 0

bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -v -c '#{pane_current_path}'
bind-key a set-window-option synchronize-panes\; display-message "synchronize-panes is now #{?pane_synchronized,on,off}"

source /usr/share/powerline/bindings/tmux/powerline.conf
set-option -g default-terminal "screen-256color"
```

### Janus Install and configure Vim
```bash
wget https://raw.githubusercontent.com/carlhuda/janus/master/bootstrap.sh
chmod +x bootstrap.sh
./bootstrap.sh
```

Add the following to `~/.vimrc.after`
```vimscript
python3 from powerline.vim import setup as powerline_setup
python3 powerline_setup()
python3 del powerline_setup
"set rtp+=/usr/share/powerline/bindings/vim/
" Always show statusline
"set laststatus=2
" " Use 256 colours (Use this setting only if your terminal supports 256
" colours)
"set t_Co=256
colorscheme Tomorrow-Night

set pastetoggle=<F2>
"let g:indentLine_color_term = 239

"No compatibility with Vi
set nocompatible

"Enable filetypes
filetype on
filetype plugin on
filetype indent on
syntax on

autocmd FileType python set omnifunc=pythoncomplete#Complete
autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
autocmd FileType html set omnifunc=htmlcomplete#CompleteTags
autocmd FileType css set omnifunc=csscomplete#CompleteCSS
autocmd FileType xml set omnifunc=xmlcomplete#CompleteTags
autocmd FileType php set omnifunc=phpcomplete#CompletePHP
autocmd FileType c set omnifunc=ccomplete#Complete

au BufNewFile,BufRead *.ejs set filetype=html

"Write the old file out when switching between files.
set autowrite

"Display current cursor position in lower right corner.
set ruler

"Want a different map leader than \
"set mapleader = ",";

"Ever notice a slight lag after typing the leader key + command? This lowers
"the timeout.
set timeoutlen=500

"Switch between buffers without saving
set hidden

"Tab stuff
set tabstop=2
set shiftwidth=2
"set softtabstop=2
"set expandtab
let g:statText="Tabs"

"Show command in bottom right portion of the screen
set showcmd

"Show lines numbers
set number

"Prefer relative line numbering?
"set relativenumber"

"Indent stuff
set smartindent
set autoindent

"Always show the status line
set laststatus=2

"Prefer a slightly higher line height
set linespace=3

"Better line wrapping
set wrap
set textwidth=79
set formatoptions=qrn1

"Set incremental searching"
set incsearch

"Highlight searching
set hlsearch

" case insensitive search
set ignorecase
set smartcase

"Hard-wrap paragraphs of text
nnoremap <leader>q gqip

"Enable code folding
set foldenable

"Hide mouse when typing
set mousehide

"Shortcut to fold tags with leader (usually \) + ft
nnoremap <leader>ft Vatzf

"Opens a vertical split and switches over (\v)
nnoremap <leader>v <C-w>v<C-w>l

"Split windows below the current window.
set splitbelow

" session settings
set sessionoptions=resize,winpos,winsize,buffers,tabpages,folds,curdir,help

"Shortcut for editing  vimrc file in a new tab
nmap <leader>ev :tabedit $MYVIMRC<cr>

"Change zen coding plugin expansion key to shift + e
let g:user_zen_expandabbr_key = '<C-e>'

"Faster shortcut for commenting. Requires T-Comment plugin
map <leader>c <c-_><c-_>

"Saves time; maps the spacebar to colon
nmap <space> :

"Automatically change current directory to that of the file in the buffer
autocmd BufEnter * cd %:p:h

"Map code completion to , + tab
imap <leader><tab> <C-x><C-o>

" More useful command-line completion
set wildmenu

"Auto-completion menu
set wildmode=list:longest

"http://vim.wikia.com/wiki/Make_Vim_completion_popup_menu_work_just_like_in_an_IDE
set completeopt=longest,menuone
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
inoremap <expr> <C-n> pumvisible() ? '<C-n>' :
  \ '<C-n><C-r>=pumvisible() ? "\<lt>Down>" : ""<CR>'
inoremap <expr> <M-,> pumvisible() ? '<C-n>' :
  \ '<C-x><C-o><C-n><C-p><C-r>=pumvisible() ? "\<lt>Down>" : ""<CR>'

"Map escape key to jj -- much faster
imap jj <esc>

"Delete all buffers (via Derek Wyatt)
nmap <silent> ,da :exec "1," . bufnr('$') . "bd"<cr>

"Bubble single lines
"http://vimcasts.org/episodes/bubbling-text/
nmap <C-Up> ddkP
nmap <C-Down> ddp

"Bubble multiple lines
vmap <C-Up> xkP`[V`]
vmap <C-Down> xp`[V`]

" Source the vimrc file after saving it. This way, you don't have to reload Vim to see the changes.
if has("autocmd")
 augroup myvimrchooks
  au!
  autocmd bufwritepost .vimrc source ~/.vimrc
 augroup END
endif

" easier window navigation
nmap <C-h> <C-w>h
nmap <C-j> <C-w>j
nmap <C-k> <C-w>k
nmap <C-l> <C-w>l

"------------------------"
"NERDTREE PLUGIN SETTINGS
"------------------------"
"Shortcut for NERDTreeToggle
nmap <leader>nt :NERDTreeToggle <CR>

"Show hidden files in NerdTree
let NERDTreeShowHidden=1

"autopen NERDTree and focus cursor in new document
"autocmd VimEnter * NERDTree
"autocmd VimEnter * wincmd p

"   Helpeful abbreviations
iab lorem Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
iab llorem Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.

"Spelling corrects. Just for example. Add yours below.
iab teh the
iab Teh The

" Get to home dir easier
" <leader>hm is easier to type than :cd ~
nmap <leader>hm :cd ~/ <CR>

" Alphabetically sort CSS properties in file with :SortCSS
:command! SortCSS :g#\({\n\)\@<=#.,/}/sort

" Shortcut to opening a virtual split to right of current pane
" Makes more sense than opening to the left
nmap <leader>bv :bel vsp

" Saves file when Vim window loses focus
au FocusLost * :wa

" Backups
"set backupdir=~/.vim/tmp/backup/ " backups
"set directory=~/.vim/tmp/swap/ " swap files
set backup " enable backup

" No more stretching for navigating files
"noremap h ;
"noremap j h
"noremap k gj
"noremap l gk
"noremap ; l

set showmatch " show matching brackets

" print empty <a> tag
map! ;h <a href=""></a><ESC>5hi

function TabTabs()
        set tabstop=4
        set shiftwidth=4
        set noexpandtab
  let g:statText="Tabs"
endfunction
nmap <F3> mz:execute TabTabs()<CR>'z

function Tab2Spaces()
        set tabstop=2
        set shiftwidth=2
        set softtabstop=2
        set expandtab
  let g:statText="2Spaces"
endfunction
nmap <F4> mz:execute Tab2Spaces()<CR>'z

function Tab4Spaces()
        set tabstop=4
        set shiftwidth=4
        set softtabstop=4
        set expandtab
  let g:statText="4Spaces"
endfunction
nmap <F5> mz:execute Tab4Spaces()<CR>'z

function TabsOrSpaces()
  return g:statText
endfunction

execute Tab2Spaces()

if ! has('gui_running')
    set ttimeoutlen=10
    augroup FastEscape
        autocmd!
        au InsertEnter * set timeoutlen=0
        au InsertLeave * set timeoutlen=1000
    augroup END
endif

function GHLink()
  " get basic info about where we are
  let branch=systemlist("git status | grep 'On branch' | cut -f3 -d ' '")[0]
  let lineno=line('.')
  let file=expand('%')

  " get the repo name from a remote
  " this will look like git@github.com:user/repo.git
  let remote=systemlist("git remote -v | grep origin | cut -f2 | cut -f1 -d ' '")[0]
  let remote=substitute(remote, 'https://', '', '')
  " get the user/repo.git part
  if remote =~ ":"
    let org_repo=split(remote, ':')[1]
    " split on / to get the user - user
    let org=split(org_repo, '/')[0]
    " and on the other side of the /, the repo name
    let repo=split(org_repo, '/')[1]
    " trim off a .git if it exists
    let repo=substitute(repo, '.git', '', '')
    " the repo url was in the remote too, between the @ and the :
    let repo_url=split(remote, ':')[0]
    let repo_url=split(repo_url, '@')[1]
  else
    let repo_url = "github.com"
    let org=split(remote, '/')[1]
    let repo=split(remote, '/')[2]
    let repo=substitute(repo, '.git', '', '')
  endif

  " the repo url was in the remote too, between the @ and the :

  " get the folder
  let path=systemlist("git rev-parse --show-prefix")[0]

  " construct, copy, and output the url
  let url='https://'.repo_url.'/'.org.'/'.repo.'/blob/'.branch.'/'.path.file.'#L'.lineno
  echom url
endfunction

command GHlink call GHLink()

" indenting guides
"let g:indent_guides_auto_colors = 0
"IndentGuidesEnable
"let g:indent_guides_guide_size = 1 " configure indent lines
"" highlight even and odd indents
"hi IndentGuidesOdd ctermbg=236
"hi IndentGuidesEven ctermbg=236

" indenting guides
let g:indentLine_color_term = 236
let g:indentLine_char = '¦'
let g:indentLine_enabled = 1
" There is a trailing space on the next line like this:
"set list lcs=tab:\¦\[[:space:]]
set list lcs=tab:\¦\
set colorcolumn=81

" python-mode
let g:pymode_folding = 0
let g:pymode_virtualenv = 0
au BufNewFile,BufRead *.py execute Tab4Spaces()

" go-mode
let g:syntastic_go_checkers = ['golint', 'govet']
let g:go_list_type = "quickfix"

let g:go_highlight_methods = 1
let g:go_highlight_functions = 1
let g:go_highlight_structs = 1
let g:go_highlight_operators = 1
let g:go_highlight_build_constraints = 1
au BufNewFile,BufRead *.go execute TabTabs()
```

Add the following to `~/.vimrc.before`
```vimscript
call janus#disable_plugin('vimwiki')
let g:vim_json_syntax_conceal = 0
let g:vim_markdown_folding_disabled = 1
let g:vim_markdown_conceal = 0
```

Configure Plugins
```bash
mkdir ~/.janus
cd ~/.janus
git clone https://github.com/Yggdroot/indentLine.git
```

For tabs vs spaces indicator add the following to: `/usr/lib/python3/dist-packages/powerline/segments/vim/__init__.py`
```
@requires_segment_info
def tabs_indicator(pl, segment_info):
	return vim.eval('TabsOrSpaces()')
```

Add the following to: `/usr/share/powerline/config_files/themes/vim/default.json`
After the paste indicator
```
{
	"function": "tabs_indicator",
	"priority": 10
},
```

Add the following to `/usr/share/powerline/config_files/colorschemes/vim/default.json`
after the paste indicator section
```
"tabs_indicator":           { "fg": "white", "bg": "gray4", "attrs": ["bold"] },
```

# Neovim

Some plugins only work with neovim, both the above vim configuration and neovim can be installed
at the same time. Neovim will be accessible with the `nvim` command.

## Install Neovim
Follow the instructions here: https://github.com/neovim/neovim?tab=readme-ov-file#install-from-source

the only change to make is:
`make -j CMAKE_BUILD_TYPE=RelWithDebInfo`

afterwards follow these instructions: https://github.com/pgosar/CyberNvim

The main config directory will now be `~/.config/nvim`

Most of the user config can be put in here: `~/.config/nvim/lua/user/user_config.lua`
```lua
-- use a user/user_config.lua file to provide your own configuration

local M = {}

-- add any null-ls sources you want here
M.setup_sources = function(b)
        return {
                b.code_actions.gitsigns,
        }
end

-- add mason sources to auto-install
M.mason_ensure_installed = {
        null_ls = {
                "stylua",
                "jq",
        },
        dap = {
                "python",
                "delve",
        },
}

-- add servers to be used for auto formatting here
M.formatting_servers = {
        ["rust_analyzer"] = { "rust" },
        ["lua_ls"] = { "lua" },
        ["null_ls"] = {
                "javascript",
                "javascriptreact",
                "typescript",
                "typescriptreact",
        },
}

-- options you put here will override or add on to the default options
M.options = {
        opt = {
                confirm = true,
        },
}

-- Set any to false that you want disabled in here.
-- take a look at the autocommands file in lua/core for more information
-- Default value is true if left blank
M.autocommands = {
        alpha_folding = true,
        treesitter_folds = true,
        trailing_whitespace = true,
        remember_file_state = true,
        session_saved_notification = true,
        css_colorizer = true,
        cmp = true,
}

-- set to false to disable plugins
-- Default value is true if left blank
M.enable_plugins = {
        -- aerial: Code outline window for skimming and quick navigation
        -- https://github.com/stevearc/aerial.nvim
        aerial = true,

        -- alpha: Customizable start screen for Neovim
        -- https://github.com/goolord/alpha-nvim
        alpha = true,

        -- autotag: Automatically close and rename HTML/XML tags
        -- https://github.com/windwp/nvim-ts-autotag
        autotag = true,

        -- bufferline: Snazzy buffer line for Neovim
        -- https://github.com/akinsho/bufferline.nvim
        bufferline = true,

        -- context: Shows the current function context in the command line
        -- https://github.com/wellle/context.vim
        context = true,

        -- dressing: Improve the default Neovim UI
        -- https://github.com/stevearc/dressing.nvim
        dressing = true,

        -- gitsigns: Git signs in the sign column
        -- https://github.com/lewis6991/gitsigns.nvim
        gitsigns = true,

        -- hop: Easy motion-like navigation
        -- https://github.com/phaazon/hop.nvim
        hop = true,

        -- img_clip: Paste images from clipboard into Neovim
        -- https://github.com/ekickx/clipboard-image.nvim
        img_clip = true,

        -- indent_blankline: Indentation guides for Neovim
        -- https://github.com/lukas-reineke/indent-blankline.nvim
        indent_blankline = true,

        -- lsp_zero: Lightweight LSP configuration for Neovim
        -- https://github.com/VonHeikemen/lsp-zero.nvim
        lsp_zero = true,

        -- lualine: Fast and easy-to-configure statusline
        -- https://github.com/nvim-lualine/lualine.nvim
        lualine = true,

        -- neodev: Neovim setup for init.lua and plugin development
        -- https://github.com/folke/neodev.nvim
        neodev = true,

        -- neoscroll: Smooth scrolling for Neovim
        -- https://github.com/karb94/neoscroll.nvim
        neoscroll = true,

        -- neotree: File explorer tree for Neovim
        -- https://github.com/nvim-neo-tree/neo-tree.nvim
        neotree = true,

        -- session_manager: Session management for Neovim
        -- https://github.com/Shatur/neovim-session-manager
        session_manager = true,

        -- noice: Highly experimental plugin that replaces UI components
        -- https://github.com/folke/noice.nvim
        noice = true,

        -- null_ls: Use Neovim as a language server to inject LSP diagnostics, code actions, and more
        -- https://github.com/jose-elias-alvarez/null-ls.nvim
        null_ls = true,

        -- autopairs: Autopairs for Neovim
        -- https://github.com/windwp/nvim-autopairs
        autopairs = true,

        -- cmp: Completion engine for Neovim
        -- https://github.com/hrsh7th/nvim-cmp
        cmp = true,

        -- colorizer: Color highlighter for Neovim
        -- https://github.com/norcalli/nvim-colorizer.lua
        colorizer = true,

        -- dap: Debug Adapter Protocol client implementation for Neovim
        -- https://github.com/mfussenegger/nvim-dap
        dap = true,

        -- notify: Fancy, configurable notification manager for Neovim
        -- https://github.com/rcarriga/nvim-notify
        notify = true,

        -- surround: Surround selections, stylishly
        -- https://github.com/kylechui/nvim-surround
        surround = true,

        -- treesitter: Nvim Treesitter configurations and abstraction layer
        -- https://github.com/nvim-treesitter/nvim-treesitter
        treesitter = true,

        -- ufo: Folding powered by lsp, treesitter and more
        -- https://github.com/kevinhwang91/nvim-ufo
        ufo = true,

        -- onedark: One Dark theme for Neovim
        -- https://github.com/navarasu/onedark.nvim
        onedark = true,

        -- project: Project management for Neovim
        -- https://github.com/ahmedkhalf/project.nvim
        project = true,

        -- rainbow: Rainbow parentheses for Neovim
        -- https://github.com/p00f/nvim-ts-rainbow
        rainbow = true,

        -- scope: Visualize and search Treesitter scopes
        -- https://github.com/tiagovla/scope.nvim
        scope = true,

        -- telescope: Highly extendable fuzzy finder over lists
        -- https://github.com/nvim-telescope/telescope.nvim
        telescope = true,

        -- toggleterm: Persist and toggle multiple terminals
        -- https://github.com/akinsho/toggleterm.nvim
        toggleterm = true,

        -- trouble: Pretty diagnostics, references, telescope results, quickfix and location list
        -- https://github.com/folke/trouble.nvim
        trouble = true,

        -- twilight: Dim inactive portions of the code you're editing
        -- https://github.com/folke/twilight.nvim
        twilight = true,

        -- whichkey: Displays a popup with possible keybindings of the command you started typing
        -- https://github.com/folke/which-key.nvim
        whichkey = true,

        -- windline: Animations for Neovim's statusline
        -- https://github.com/windwp/windline.nvim
        windline = true,

        -- zen: Distraction-free coding for Neovim
        -- https://github.com/folke/zen-mode.nvim
        zen = true,
}

-- add extra plugins in here
M.plugins = {
        {
                "nvim-neotest/neotest",
                dependencies = {
                        "nvim-lua/plenary.nvim",
                        "nvim-treesitter/nvim-treesitter",
                        "antoinemadec/FixCursorHold.nvim",
                },
        },
  { 'knsh14/vim-github-link', lazy = false },
  { 'ctrlpvim/ctrlp.vim', lazy = false },
  { 'tpope/vim-fugitive', lazy = false },
  { 'editorconfig/editorconfig-vim', lazy = false },
}

M.lsp_config = {
        clangd = {},
}

-- add extra configuration options here, like extra autocmds etc.
-- feel free to create your own separate files and require them in here
M.user_conf = function()
        vim.cmd([[
  autocmd VimEnter * lua vim.notify("Welcome to CyberNvim!", "info", {title = "Neovim"})]])
  vim.cmd([[
    function TabTabs()
            set tabstop=4
            set shiftwidth=4
            set noexpandtab
      let g:statText="Tabs"
    endfunction
    nmap <F3> mz:execute TabTabs()<CR>'z

    function Tab2Spaces()
            set tabstop=2
            set shiftwidth=2
            set softtabstop=2
            set expandtab
      let g:statText="2Spaces"
    endfunction
    nmap <F4> mz:execute Tab2Spaces()<CR>'z

    function Tab4Spaces()
            set tabstop=4
            set shiftwidth=4
            set softtabstop=4
            set expandtab
      let g:statText="4Spaces"
    endfunction
    nmap <F5> mz:execute Tab4Spaces()<CR>'z
    function TabsOrSpaces()
            echo g:statText
    endfunction

    execute Tab2Spaces()
  ]])

  vim.cmd("au BufNewFile,BufRead *.py execute Tab4Spaces()")
  vim.cmd("au BufNewFile,BufRead *.go execute TabTabs()")
  local builtin = require('telescope.builtin')
  vim.keymap.set('n', '<leader>ff', builtin.find_files, { desc = 'Telescope find files' })
  vim.keymap.set('n', '<leader>fg', builtin.live_grep, { desc = 'Telescope live grep' })
  vim.keymap.set('n', '<leader>fb', builtin.buffers, { desc = 'Telescope buffers' })
  vim.keymap.set('n', '<leader>fh', builtin.help_tags, { desc = 'Telescope help tags' })
  vim.keymap.set('n', '<leader>m', ":let &mouse = &mouse == 'a' ? '' : 'a'<cr>")

end

return M
```

any additional nvim plugins can be added to the section where it says `M.plugins` it's a lua table.
like this one: `{ 'tpope/vim-fugitive', lazy = false },`

just add it on a new line

this is using the `lazy.nvim` plugin manager. After adding a plugin, restart nvim.

Suprisingly vim is useful to use to edit the nvim config. So you can keep working on it without
breaking anything.

### Neovim tips

The `<leader>` key is the spacebar by default for CyberNvim. Normally it's `\` in vim.
The filenames show up at the top, they are actually buffers and you can navigate them with the
`]b` and `[b` commands, as opposed to `gt` and `gT` which still work but those are for tabs, which
are containers for buffers.

Mouse is set to on, this can be turned off with `:set mouse=` and back on with `:set mouse=nvi`

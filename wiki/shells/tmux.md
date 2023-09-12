# TMux

[https://gist.github.com/MohamedAlaa/2961058](https://gist.github.com/MohamedAlaa/2961058)

## Sessions

```bash
tmux new
tmux new -s mysession                # specify session name
tmux new -n mytopwindow top          # specify window name
tmux new-session -d -n mysession     
tmux attach -t mysession             # the name of a session
tmux attach -dt mysession            # detach other client from this session
tmux ls
```

<table><thead><tr><th width="239">Shortcut</th><th width="160">Command</th><th>Description</th></tr></thead><tbody><tr><td>C-b ?</td><td></td><td>List key bindings</td></tr><tr><td>C-b c</td><td></td><td>New window</td></tr><tr><td>C-b : </td><td></td><td>Interactive command prompt</td></tr><tr><td>C-b d</td><td></td><td>Detach sessions</td></tr><tr><td></td><td>kill-server</td><td>Kill the tmux server</td></tr><tr><td>C-b c</td><td>new-window<br>neww</td><td>Create a window</td></tr><tr><td>C-b %</td><td>split-window -h</td><td>Window horizontal split</td></tr><tr><td>C-b "</td><td>split-window -v</td><td>Window vertical split</td></tr><tr><td>C-b 0</td><td>select-window</td><td>Change the current window</td></tr><tr><td>C-b '</td><td>...</td><td>Change window by prompting index</td></tr><tr><td>C-b n</td><td>...</td><td>Change window to the new index</td></tr><tr><td>C-b p</td><td>...</td><td>Change to the previous window</td></tr><tr><td>C-b l</td><td>...</td><td>Change to the last window</td></tr><tr><td>C-b Up|Left|Right|Down</td><td>select-pane</td><td>Change the pane</td></tr><tr><td>C-b q 1</td><td>display-panes</td><td>Show pane indexes then choose pane 1</td></tr><tr><td>C-b o</td><td></td><td>Move to the next pane</td></tr><tr><td>C-b s</td><td></td><td>Show sessions with current selected</td></tr><tr><td>C-b w</td><td></td><td>Show expanded sessions</td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

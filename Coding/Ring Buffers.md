tags: #ring_buffer

As it seems I am implementing ring buffers over and over again, I want to collect them here to focus a bit of best practice

#### `dareplane_data_monitor`
```python
    def ring_line_update(self, lines: np.ndarray):
        # the cut if necessary
        from_end = max(lines.shape[1] - (self.nbuffer - self.icurr), 0)
        LOGGER.debug(f"{from_end=}, {lines.shape=}")

        # no split necessary, everything still fits
        if from_end == 0:

            new_end = self.icurr + lines.shape[1]
            self.lines[:, self.icurr:new_end, :] = lines
            self.icurr = new_end

        else:
            # split is necessary, if from_end == lines.shape[1] --> data is only at the beginning   # noqa
            if from_end != lines.shape[1]:
                endview = lines[:, :-from_end, :]
                new_end = self.icurr + endview.shape[1]
                self.lines[:, self.icurr:new_end, :] = endview

                # Will always be at max
                self.icurr = new_end
            if from_end > 0:
                # Overflow to the beginning
                startview = lines[:, -from_end:, :]
                self.lines[:, :from_end, :] = startview
                self.icurr = startview.shape[1]
```
Where this is a buffer for multiple lines within a numpy array of shape `n_lines, n_data_point, 2`, so a collection of x-y coordinates for each line.

In the same project, I have another implementation on the side of the  `StreamWatchers` which looks like this:
```python
    def add_samples(self, samples: list, times: list):
        if len(samples) > 0 and len(times) > 0:

            # make it a ring buffer with FIFO
            old_i = self.curr_i
            self.curr_i = (self.curr_i + len(samples)) % self.n_buffer

            # plain forward fill
            if old_i < self.curr_i:
                self.buffer[old_i:self.curr_i] = samples
                self.buffer_t[old_i:self.curr_i] = times

            # split needed -> start over at beginning
            else:
                nfull = self.n_buffer - old_i
                self.buffer[old_i:] = samples[:nfull]
                self.buffer_t[old_i:] = times[:nfull]

                self.buffer[:self.curr_i] = samples[nfull:]
                self.buffer_t[:self.curr_i] = times[nfull:]

            self.last_t = times[-1]
```
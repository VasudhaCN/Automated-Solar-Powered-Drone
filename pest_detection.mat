dataset_path = 'C:\Users\Narasimhan\Downloads\archive\farm_insects\Armyworms_small';
image_files = dir(fullfile(dataset_path, '*.jpg'));  % Change if using .png

output_folder = 'C:\Users\Narasimhan\Downloads\archive\farm_insects\Armyworms_1';
if ~exist(output_folder, 'dir')
    mkdir(output_folder);
end

total_silhouette = 0;
count = 0;

for i = 1:length(image_files)
    % Read and resize image
    img = imread(fullfile(dataset_path, image_files(i).name));
    img_resized = imresize(img, [256, 256]);

    % Reshape image to 2D pixel list (R,G,B)
    pixel_data = double(reshape(img_resized, [], 3));

    % K-means clustering
    K = 2;
    [cluster_idx, ~] = kmeans(pixel_data, K, 'MaxIter', 1000);

    % Reshape cluster indices to image size
    clustered_img = reshape(cluster_idx, size(img_resized, 1), size(img_resized, 2));

    % Convert to grayscale for brightness-based pest detection
    gray_img = rgb2gray(img_resized);

    % Calculate average brightness for each cluster
    avg_brightness = zeros(K, 1);
    for k = 1:K
        avg_brightness(k) = mean(gray_img(clustered_img == k));
    end

    % Assume pest cluster has lowest brightness
    [~, pest_cluster] = min(avg_brightness);
    pest_mask = clustered_img == pest_cluster;

    % Pest localization
    stats = regionprops(pest_mask, 'BoundingBox', 'Centroid', 'Area');

    figure('Visible','off'); 
    imshow(img_resized);
    hold on;
    for k = 1:length(stats)
        rectangle('Position', stats(k).BoundingBox, 'EdgeColor', 'r', 'LineWidth', 2);
        plot(stats(k).Centroid(1), stats(k).Centroid(2), 'bo');
    end
    hold off;
    saveas(gcf, fullfile(output_folder, ['pest_localized_' image_files(i).name]));
    close;

    % Silhouette Score Calculation
    sample_size = min(2000, size(pixel_data,1));
    idx_sample = randperm(size(pixel_data,1), sample_size);
    sample_data = pixel_data(idx_sample, :);
    sample_labels = cluster_idx(idx_sample);

    sil_scores = silhouette(sample_data, sample_labels);
    avg_silhouette = mean(sil_scores);
    total_silhouette = total_silhouette + avg_silhouette;
    count = count + 1;
end
overall_avg_silhouette = total_silhouette / count;
fprintf('Overall Average Silhouette Score: %.4f\n', overall_avg_silhouette);
